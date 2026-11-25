# ReWear
**Course:** Electronic Business Development (BINF 503) · Winter 2026
**Instructor:** Dr. Shaimaa Masry
**Teaching Assistants:** Mr. Ahmed 

## 1. Team

| Name | Student ID | Tutorial group | GitHub username |
|---|---|---|---|
| Rawan Elshimey | 22002528 | T03 | rawanelshimey-svg |
| Habiba Mostafa | 22002476 | T04 | habibaelkabany13-cmyk |
| Amina Hashad | 22002461 | T04 | hashadamina |
| Halla Maged | 22002479 | T03 | hallamaged20 |
| Ahmed Tamer | 22002455 | T03 | ahmedtamerali06 |
## 2. The idea

ReWear is a sustainable online clothing exchange platform that allows users to swap pre-owned clothes instead of purchasing new ones. It targets students and individuals who want to refresh their wardrobes affordably while reducing textile waste. Users can upload clothing items, discover clothes based on their preferences, save favourites, send swap requests and complete exchanges. ReWear aims to make sustainable fashion more accessible, affordable and convenient.

## 3. Features

### 3.1 The full product

- User registration, login and profile management.
- Upload, edit and delete clothing listings.
- Browse, search and filter available clothing.
- Save favourite clothing items to a personal wishlist.
- Organize saved items into custom collections.
- Personalized clothing discovery based on user preferences.
- Send, edit and cancel clothing swap requests.
- Accept or reject incoming swap requests.
- Complete exchanges and view exchange history.
- Rate and review users after completed exchanges.
- Real-time messaging between users (future feature).
- Notifications for swap requests and updates (future feature).
- Delivery and shipping coordination (future feature).
- Environmental impact tracking (future feature).

### 3.2 The use cases we will build

| # | Use case | What the user does | What they see | What is stored |
|---|---|---|---|---|
| 1 | Registration and login | Registers with name, email and password, logs in and updates their profile | A personalized home page and profile | User information with a hashed password |
| 2 | Clothing listing management | Uploads, edits, deletes, searches and filters clothing listings | Available clothing and their personal wardrobe | Clothing details, image URL, owner and availability status |
| 3 | Wishlist and personalized clothing discovery | Saves and removes favourite items, creates custom collections and updates clothing preferences | Personal collections and clothing matching their preferences | Wishlist items, collections and user preferences |
| 4 | Swap request management | Selects clothing to exchange, sends swap requests, edits or cancels pending requests | Sent swap requests and their current status | Offered item, requested item, sender, receiver, message and request status |
| 5 | Exchange management and reviews | Accepts or rejects incoming requests, completes exchanges and submits ratings and reviews | Incoming requests, exchange history and user reviews | Updated exchange status, clothing availability, ratings and reviews |

## 4. Who owns what

| Member | Use case |
|---|---|
| Halla Maged | 1. Registration and login |
| Habiba Mostafa | 2. Clothing listing management |
| Ahmed Tamer | 3. Wishlist and personalized clothing discovery |
| Amina Hashad | 4. Swap request management |
| Rawan Elshimey | 5. Exchange management and reviews |
## 5. Data model

### User

```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  bio: { type: String, default: "" },
  profileImage: { type: String, default: "" }
}, { timestamps: true });
```

### Clothing

```js
const clothingSchema = new mongoose.Schema({
  owner: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  title: { type: String, required: true },
  description: { type: String, required: true },
  category: { type: String, required: true },
  size: { type: String, required: true },
  style: { type: String, required: true },
  condition: { type: String, required: true },
  imageUrl: { type: String, required: true },
  status: {
    type: String,
    enum: ["available", "reserved", "swapped"],
    default: "available"
  }
}, { timestamps: true });
```

### Wishlist

```js
const wishlistSchema = new mongoose.Schema({
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  clothing: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Clothing",
    required: true
  }
}, { timestamps: true });

wishlistSchema.index({ user: 1, clothing: 1 }, { unique: true });
```

### Collection

```js
const collectionSchema = new mongoose.Schema({
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  name: { type: String, required: true },
  description: { type: String, default: "" },
  items: [{
    type: mongoose.Schema.Types.ObjectId,
    ref: "Clothing"
  }]
}, { timestamps: true });
```

### Preference

```js
const preferenceSchema = new mongoose.Schema({
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true,
    unique: true
  },
  sizes: [String],
  categories: [String],
  styles: [String]
}, { timestamps: true });
```

### SwapRequest

```js
const swapRequestSchema = new mongoose.Schema({
  requester: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  receiver: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  offeredClothing: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Clothing",
    required: true
  },
  requestedClothing: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "Clothing",
    required: true
  },
  message: { type: String, default: "" },
  status: {
    type: String,
    enum: ["pending", "accepted", "rejected", "cancelled", "completed"],
    default: "pending"
  }
}, { timestamps: true });
```

### Review

```js
const reviewSchema = new mongoose.Schema({
  swap: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "SwapRequest",
    required: true
  },
  reviewer: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  reviewedUser: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
    required: true
  },
  rating: {
    type: Number,
    required: true,
    min: 1,
    max: 5
  },
  comment: { type: String, default: "" }
}, { timestamps: true });

reviewSchema.index({ swap: 1, reviewer: 1 }, { unique: true });
```


## 6. Run it locally

> Leave this section as it is until Submission 2 (Thursday 10 December 2026,
> 23:59). Then fill it in: we follow these steps to run your project.

### Back end

```bash
cd backend
npm install
# create backend/.env from backend/.env.example (see the table below)
npm start
```

The API runs on http://localhost:3000.

| Variable in `.env` | What it is |
|---|---|
| `MONGODB_URI` | The connection string of your MongoDB Atlas database |
| `JWT_SECRET` | Any long random text, used to sign login tokens |
| [Any other variable] | [What it is] |

### Front end

```bash
cd frontend
npm install
npm run dev
```

The app runs on [the address `npm run dev` prints, e.g. http://localhost:5173].
