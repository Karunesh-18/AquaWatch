# AquaWatch – Water Quality Classification

Predicts water quality category (Good / Moderate / Poor) from pH, turbidity (NTU), temperature (°C), and dissolved oxygen (mg/L). Built with a scikit-learn model, FastAPI backend, and a sleek water-themed HTML/CSS UI.

## Project Structure
- `notebooks/AquaWatch_Water_Quality_Model.ipynb` – trains and evaluates the classifier; exports model.
- `backend/app.py` – FastAPI prediction API with confidence scores and CORS enabled.
- `backend/requirements.txt` – dependencies for backend and notebook.
- `frontend/index.html`, `styles.css`, `app.js` – modern water-themed UI.
- `Kaggle.json` – credentials for Kaggle API (already provided).

## Prerequisites
- Python 3.10+ recommended.
- Valid Kaggle credentials in `w:\VSCODE\AquaWatch\Kaggle.json`.

## Train the Model
1. Open `notebooks/AquaWatch_Water_Quality_Model.ipynb` in Jupyter.
2. Optional: set `kaggle_slug` to a dataset that includes columns `ph`, `turbidity`, `temperature`, `dissolved_oxygen`. If not provided or missing, the notebook simulates realistic data and labels via a WQI-like heuristic.
3. Run all cells. The notebook prints accuracy, classification report, and confusion matrix; then saves:
   - Model: `backend/aquawatch_model.joblib`
   - Class names: `backend/class_names.json`

## Run the Backend API
```bash
python -m pip install -r w:\VSCODE\AquaWatch\backend\requirements.txt
python -m uvicorn backend.app:app --host 0.0.0.0 --port 8000
```
API will be available at `http://localhost:8000/`.

### Example Prediction Request
POST `http://localhost:8000/predict`
```json
{
  "ph": 7.2,
  "turbidity": 3.5,
  "temperature": 18.0,
  "dissolved_oxygen": 7.5
}
```
Response includes `category`, `confidence`, and per-class probabilities.

## Run the UI
Use a simple static server and open in browser:
```bash
python -m http.server 5500 -d w:\VSCODE\AquaWatch\frontend
```
Visit `http://localhost:5500/`. Enter parameters and click "Predict Quality".

## Notes
- If no trained model is present, the API uses a safe rule-based fallback to provide a category and confidence (derived from a WQI-like score). Train the model for learned predictions.
- The UI uses a water palette (teal & deep blues) and shows confidence and probabilities.
- Keep Kaggle API key secure. The notebook sets `KAGGLE_CONFIG_DIR` to `w:\VSCODE\AquaWatch` so the provided `Kaggle.json` is used automatically.