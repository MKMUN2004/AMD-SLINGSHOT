# VitaLens

**AI-powered nutrition guidance, built for real eating habits.**

VitaLens is a personal nutrition assistant that understands the food you actually eat — not just what's in a Western calorie database. Powered by Google Gemini, it combines science-backed goal tracking with conversational AI to help you eat better, consistently.

---

## What it does

- **Food analysis** — Describe a meal in plain language and get an instant nutritional breakdown: calories, macros, micronutrients, health score, and smarter alternatives.
- **Meal planning** — Generate weekly meal plans tailored to your goals, dietary preferences, and cuisine.
- **Nutrition chat** — Ask anything. Get evidence-based answers from an AI that understands regional and cultural foods.
- **Daily tracking** — Log meals and water, monitor your intake against targets, and maintain eating streaks.
- **Grocery lists** — Auto-generate categorized shopping lists from your meal plan, with estimated costs.
- **Goal science** — Calorie and macro targets derived from your BMR and TDEE using the Mifflin-St Jeor equation.

---

## Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.11, Flask |
| AI | Google Gemini 2.0 Flash |
| Frontend | HTML5, CSS3, Vanilla JS |
| Deployment | Docker, Google Cloud Run |
| Testing | Pytest |

---

## Getting started

**Requirements:** Python 3.11+, a [Gemini API key](https://aistudio.google.com/apikey)

```bash
git clone https://github.com/your-username/vitalens.git
cd vitalens

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt

cp .env.example .env
# Add your GOOGLE_API_KEY to .env

python app.py
# Running at http://localhost:8080
```

### Environment variables

| Variable | Required | Notes |
|---|---|---|
| `GOOGLE_API_KEY` | Yes | Gemini API key |
| `SECRET_KEY` | No | Auto-generated if omitted |
| `PORT` | No | Defaults to 8080 |
| `FLASK_ENV` | No | `development` or `production` |

---

## Project structure

```
vitalens/
├── app.py                  # Flask app — API endpoints and page routes
├── requirements.txt
├── Dockerfile
├── .env.example
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── dashboard.html
│   ├── chat.html
│   ├── meal_plan.html
│   ├── analyze.html
│   ├── profile.html
│   ├── grocery.html
│   └── 404.html
├── static/
│   ├── css/style.css
│   └── js/app.js
└── tests/
    └── test_app.py
```

---

## API reference

### Pages

| Route | Description |
|---|---|
| `GET /` | Landing page |
| `GET /dashboard` | Daily tracking |
| `GET /chat` | AI nutrition chat |
| `GET /meal-plan` | Meal plan generator |
| `GET /analyze` | Food analyzer |
| `GET /profile` | Health profile |
| `GET /grocery` | Grocery list |

### Endpoints

| Method | Route | Description |
|---|---|---|
| `POST` | `/api/profile` | Save profile, calculate BMR/TDEE |
| `GET` | `/api/profile` | Retrieve current profile |
| `POST` | `/api/chat` | Send message to AI nutritionist |
| `POST` | `/api/analyze` | Analyze a food item |
| `POST` | `/api/meal-plan` | Generate a meal plan |
| `POST` | `/api/log-meal` | Log a meal |
| `GET` | `/api/daily-summary` | Today's nutrition totals |
| `POST` | `/api/water` | Log water intake |
| `GET` | `/api/quick-tip` | Get a daily nutrition tip |
| `POST` | `/api/grocery-list` | Generate a grocery list |
| `GET` | `/health` | Health check (Cloud Run) |

---

## Testing

```bash
pytest tests/ -v
```

22 tests covering health checks, all page routes, profile CRUD, BMR calculation, meal logging, water tracking, chat validation, food analysis, and tip generation.

---

## Deployment

### Google Cloud Run

```bash
gcloud builds submit --tag gcr.io/PROJECT_ID/vitalens

gcloud run deploy vitalens \
  --image gcr.io/PROJECT_ID/vitalens \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated \
  --set-env-vars GOOGLE_API_KEY=your-key-here
```

### Docker (local)

```bash
docker build -t vitalens .
docker run -p 8080:8080 -e GOOGLE_API_KEY=your-key vitalens
```

---

## Design notes

- **Session storage over a database** — zero setup for evaluators; data is ephemeral by design.
- **Gemini 2.0 Flash** — optimized for low-latency responses in real-time chat and analysis flows.
- **Vanilla CSS** — no build step, full control, keeps the repo lightweight.
- **Gunicorn** with 2 workers handles concurrent requests without added infrastructure.

---

## Assumptions

- An internet connection is required for all AI features (Gemini API calls).
- Nutritional output is approximate and informational — not a substitute for professional dietary advice.
- Session data resets on server restart; this is intentional for privacy.

---

## License

MIT — see [LICENSE](LICENSE) for details.
