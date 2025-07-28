## DEPLOYING A RAILS API (Render) + REACT/VITE FRONTEND (Netlify)  

---

### 1 INTRODUCTION  

**What's "deployment"?**  
Moving your code from a laptop to public servers so anyone with a browser can use it.

**Why these two platforms?**  
* **Render** hosts back-end services and a fully-managed PostgreSQL database—both free-tier friendly.  
* **Netlify** specialises in ultra-fast static front-ends with a global CDN, instant rollbacks, and zero-config HTTPS.

**What's a database?**  
Think of it as a digital filing cabinet that stores users, orders, posts—anything your app needs to remember.

**Why PostgreSQL?**  
PostgreSQL (pronounced *post-gress-Q-L*) is a powerful, open-source, relational database that plays nicely with Rails.

---

### 2 PREREQUISITES  

* GitHub account with two repos (backend & frontend).  
* Rails 6, 7, or 8 API-only app already using **Postgres locally** (`gem "pg"` in the Gemfile, **no** `sqlite3`, and `adapter: postgresql` in `config/database.yml`).  
* React app scaffolded with **Vite** (the class default).  
* Comfort with the terminal and Git basics.

---

### 3 CREATE YOUR PRODUCTION DATABASE  

1. Go to **Render.com** → **Sign Up with GitHub** → Authorise.  
2. Click **New** → **PostgreSQL**.  
3. Fill the form:  
   * **Name** → `mini-capstone-db` (or your project name)  
   * **Region** → Select the region nearest to you  
   * **Instance Type** → Free  
4. Click **Create Database** → build ≈ 60 s.  
5. Once created, copy the **Internal Database URL** (you'll need this for the web service).  

---

### 4 CREATE YOUR RAILS WEB SERVICE (API)  

1. Go to **Render** → **New** → **Web Service**.  
2. Select **Ruby** for the language.  
3. Connect your Rails repository.  
4. Configure the service:

| Field | Value |
|-------|-------|
| Name | `mini-capstone-api` (or your project name) |
| Branch | `main` |
| Language | Ruby |
| Build Command | `bundle install; bundle exec rails db:create db:migrate;` |
| Start Command | `bundle exec puma -C config/puma.rb -b tcp://0.0.0.0:$PORT` |
| Instance Type | Free |

5. **Environment Variables**:

| Key | Value |
|-----|-------|
| `DATABASE_URL` | *Internal Database URL from your PostgreSQL database* |
| `RAILS_MASTER_KEY` | *Contents of your local `config/master.key` file* |

6. Click **Create Web Service** → build & deploy (~5 min).

---

### 5 SET UP YOUR RAILS API LOCALLY  

#### 5.1 Add CORS Gem to Gemfile  

```ruby
# Gemfile
gem "rack-cors"
```

Run `bundle install` after adding this.

#### 5.2 Configure CORS (Temporarily)  

`config/initializers/cors.rb`

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "http://localhost:5173"
    resource "*",
             headers: :any,
             methods: %i[get post put patch delete options head],
             credentials: true
  end
end
```

> **Note**: This is a temporary setup for local development. We'll update this with the actual Netlify URL after deploying the frontend.

#### 5.3 Configure Database.yml  

Update `config/database.yml` production section:

```yaml
production:
  <<: *default
  url: <%= ENV["DATABASE_URL"] %>

cable:
  <<: *default
  url: <%= ENV["DATABASE_URL"] %>

queue:
  <<: *default
  url: <%= ENV["DATABASE_URL"] %>

cache:
  <<: *default
  url: <%= ENV["DATABASE_URL"] %>
```

**What this does:**
- **`production:`** - Main database configuration for your Rails app when deployed *(this is the one we're actually using)*
- **`cable:`** - Database for ActionCable (WebSocket connections) *(not used in our demo app, but good to configure)*
- **`queue:`** - Database for background job queues (like Sidekiq) *(not used in our demo app, but good to configure)*
- **`cache:`** - Database for Rails cache storage *(not used in our demo app, but good to configure)*

> **Note**: Our mini capstone demo only uses the `production` database, but configuring all four prevents errors if you add these features later. Rails will only connect to the databases it actually needs.

**Why `ENV["DATABASE_URL"]`:**
- **Security**: Never hardcode database credentials in your code
- **Flexibility**: Uses the environment variable we set in Render
- **Rails Standard**: Rails automatically parses this URL format for connection details

**Why `<<: *default`:**
- **Inheritance**: Merges in default settings (like adapter, encoding, pool size)
- **DRY Principle**: Avoids repeating common configuration across environments

---

### 6 TEST YOUR DEPLOYED API  

1. After the build completes, go to `<your-deployed-api-url>/products.json`.  
2. You should see an empty array `[]` (no data seeded yet).

---

### 7 ADD SEED DATA  

#### 7.1 Update your seeds.rb  

> **Note**: The following seed data is specific to the demo mini capstone app we've been building together. This section is **optional** and depends on your specific app structure. Modify or skip this section based on your app's models and requirements.

Replace the contents of `db/seeds.rb` with comprehensive seed data:

```ruby
# db/seeds.rb
# ------------------------------------------------------------------
# Populates:
#   • 1 admin + 10 regular users
#   • 3 suppliers
#   • 10 categories
#   • 50 products (each with 1-3 images and 1-3 categories)
#   • 100 orders (random users & products, qty 1-5)
#
# Requires faker. If it isn't in your Gemfile yet, add:
#   gem "faker", "~> 3.4"
# then: bundle install
# ------------------------------------------------------------------

require "faker"
require "bigdecimal/util"

TAX_RATE = 0.05 # 5 %

puts "\n🌱  Resetting tables…"
[Image, CategoryProduct, Category, CartedProduct, Order,
 Product, Supplier, User].each(&:delete_all)

# ---------------------------- Users -------------------------------
puts "🔑  Creating users…"
admin = User.create!(
  name:  "Admin User",
  email: "admin@example.com",
  password:              "password",
  password_confirmation: "password",
  admin: true
)

10.times do
  User.create!(
    name:  Faker::Name.name,
    email: Faker::Internet.unique.email,  # works on any Faker version
    password:              "password",
    password_confirmation: "password"
  )
end

# -------------------------- Suppliers -----------------------------
puts "🏭  Creating suppliers…"
3.times do
  Supplier.create!(
    name:         Faker::Company.unique.name,
    email:        Faker::Internet.unique.email,
    phone_number: Faker::PhoneNumber.phone_number
  )
end
suppliers = Supplier.all

# -------------------------- Categories ----------------------------
puts "🏷️   Creating categories…"
category_names = %w[
  Apparel Electronics Books Home Beauty Toys
  Sports Grocery Automotive Garden
]
categories = category_names.map { |n| Category.create!(name: n) }

# --------------------------- Products -----------------------------
puts "📦  Creating products, images & tags…"
50.times do
  product = Product.create!(
    name:        Faker::Commerce.unique.product_name,
    description: Faker::Lorem.sentence(word_count: 12),
    price:       Faker::Commerce.price(range: 5..200).round, # integer dollars
    supplier:    suppliers.sample
  )

  # 1-3 placeholder images
  rand(1..3).times do
    Image.create!(
      url: "https://picsum.photos/seed/#{SecureRandom.hex(4)}/400/400",
      product: product
    )
  end

  # tag with 1-3 categories
  categories.sample(rand(1..3)).each do |cat|
    CategoryProduct.create!(product: product, category: cat)
  end
end
products = Product.all

# ---------------------------- Orders ------------------------------
puts "🧾  Creating orders…"
users = User.where(admin: false)

100.times do
  user = users.sample
  
  # Create 1-3 carted products for this order
  carted_products = []
  rand(1..3).times do
    product = products.sample
    quantity = rand(1..5)
    
    carted_products << CartedProduct.create!(
      user: user,
      product: product,
      quantity: quantity,
      status: "purchased"
    )
  end
  
  # Calculate totals for the order
  subtotal = carted_products.sum { |cp| (cp.product.price * cp.quantity).to_d }
  tax = (subtotal * TAX_RATE).round(2)
  total = subtotal + tax
  
  # Create the order
  order = Order.create!(
    user: user,
    subtotal: subtotal,
    tax: tax,
    total: total
  )
  
  # Associate the carted products with the order
  carted_products.each do |cp|
    cp.update!(order: order)
  end
end

# --------------------------- Summary ------------------------------
puts "\n✅  Seeding complete!"
puts "   Users:         #{User.count} (#{User.where(admin: true).count} admin)"
puts "   Suppliers:     #{Supplier.count}"
puts "   Categories:    #{Category.count}"
puts "   Products:      #{Product.count}"
puts "   Images:        #{Image.count}"
puts "   Orders:        #{Order.count}"
puts "   Cart Items:    #{CartedProduct.count}"

# If you ever need to reuse Faker's unique generators elsewhere:
#   Faker::UniqueGenerator.clear
```

#### 7.2 Update Build Command with Seeding  

> **Note**: Only follow this step if you're using seed data. If you don't have seeds or prefer to add data manually, keep the build command as `bundle install; bundle exec rails db:create db:migrate;`

1. Go to **Render** → your web service → **Settings**.  
2. Update the **Build Command** to:  
   ```
   bundle install; bundle exec rails db:create db:migrate db:seed;
   ```
3. **Save** the changes → Render will automatically rebuild and deploy with the new command.  

> **Important**: You may want to remove `db:seed` from the build command after the first successful deploy to avoid re-seeding on every deployment.

---

### 8 DEPLOY YOUR FRONTEND  

#### 8.1 Deploy to Netlify  

1. Go to **Netlify.com** → **Sign Up with GitHub**.  
2. Click **Add New Project** → **Import an Existing Project**.  
3. Select your frontend repository.  
4. Configure the build settings:

| Field | Value |
|-------|-------|
| Branch | `main` |
| Build Command | `npm run build` |
| Publish Directory | `dist` |

5. Click **Deploy Site** → build ≈ 1 min.  
6. Netlify will assign a URL like `https://sparkling-dugong-12345.netlify.app`.

#### 8.2 Configure Frontend API URL  

**For Bolt Frontend** (update `api.ts`):  
```typescript
const API_BASE_URL = import.meta.env.MODE === "development" ? "http://localhost:3000" : "<your-mini-capstone-url>";
```

**For Original Mini Capstone** (add to `main.jsx` or `app.jsx`):  
```javascript
axios.defaults.baseURL = process.env.NODE_ENV === "development" ? "http://localhost:3000" : "<your-backend-url>";
```

Replace `<your-mini-capstone-url>` or `<your-backend-url>` with your actual Render API URL.

#### 8.3 Check for Hardcoded API URLs

> **Important**: After configuring the API URL above, make sure you don't have any hardcoded `http://localhost:3000` URLs remaining in your React components.

**Search your frontend code for hardcoded URLs:**
- **Mac**: `Shift + Cmd + F` to open VS Code's search
- **Windows**: `Shift + Ctrl + F` to open VS Code's search
- Search for: `localhost:3000`
- Make sure to search in your entire frontend project folder

**Common places to check:**
- Individual component files making axios calls
- Service files or API utilities
- Any fetch() or axios.get/post/put/delete calls

**Fix any hardcoded URLs you find:**
```javascript
// ❌ Bad - hardcoded localhost
axios.get("http://localhost:3000/products")

// ✅ Good - uses configured base URL
axios.get("/products")  // Will use the baseURL we configured above
```

#### 8.4 Update CORS with Final Configuration  

Now that you have your actual Netlify URL, update `config/initializers/cors.rb` with the complete CORS configuration:

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "https://your-actual-netlify-url.netlify.app", "http://localhost:5173"
    resource "*",
             headers: :any,
             methods: %i[get post put patch delete options head],
             credentials: true
  end
end
```

Replace `https://your-actual-netlify-url.netlify.app` with your actual Netlify URL.

#### 8.5 Deploy the Changes  

1. **Commit and push your frontend changes** (API URL configuration) to GitHub → Netlify will automatically redeploy.  
2. **Commit and push your backend changes** (CORS configuration) to GitHub → Render will automatically redeploy.  
3. Wait for both deployments to complete before testing.

---

### 9 OPTIONAL CONFIGURATIONS  

#### 9.1 Custom Domain  

Netlify → **Domain Settings** → add or purchase your domain.

#### 9.2 Remove Seeding from Build Command  

After your first successful deploy with data:  

1. Go to **Render** → your web service → **Settings**.  
2. Update **Build Command** back to:  
   ```
   bundle install; bundle exec rails db:create db:migrate;
   ```

This prevents re-seeding on every deployment.

---

### 10 TEST END-TO-END  

1. Open the Netlify URL—frontend should load instantly.  
2. Test all features that make API calls.  
3. Check for errors:  
   * Browser console & Network tab (CORS, 404, 500).  
   * Render → Service → Logs.  
   * Netlify → Site → Functions (if using).

---

### 11 CONTINUOUS DEPLOYMENT FLOW  

* Push to **main** in either repo → the corresponding platform rebuilds.  
* Rails migrations run automatically.  
* Netlify keeps previous deploys—roll back in one click if needed.  
* You can **Pause** free Render services to conserve monthly hours.

---

## QUICK TROUBLESHOOTING TABLE  

| Issue | Fix |
|-------|-----|
| Frontend shows CORS error | Update `cors.rb` origins list with exact Netlify URL (https, no trailing slash). |
| Netlify build fails: "vite: not found" | Ensure `vite` is in devDependencies and `npm run build` works locally. |
| Render build fails: "pg not found" | Gemfile must include `pg`; remove `sqlite3`. |
| Render build fails: "faker not found" | Add `gem "faker", "~> 3.4"` to Gemfile. |
| Render returns 502 / Health check failed | Check logs: missing `RAILS_MASTER_KEY`, unsupported gem, or app not binding to `$PORT`. |
| React refresh 404 | Add Netlify rewrite rule `/* /index.html 200`. |
| First API call is slow | Free Postgres cold-start—expect ~30s. Paid tier removes this. |
| Empty array from API | Seeds haven't run yet; update build command to include `db:seed`. |
| API calls still go to localhost | Check for hardcoded `localhost:3000` URLs in components; use `axios.defaults.baseURL` instead. |

---

### CONGRATULATIONS 🎉  

You now have a **Rails API + PostgreSQL** live on Render **and** a **React/Vite** frontend on Netlify—fully integrated with seed data, automatically redeploying, and ready for demos or real users. Keep shipping code and watch your updates go live within minutes!