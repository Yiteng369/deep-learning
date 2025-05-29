
## 📚 Overview of the Experimental Framework

The project is divided into **four stages**, each addressing a specific research question and modeling scenario:

| Stage | Title                                  | Key Goal                                                                 |
|-------|----------------------------------------|--------------------------------------------------------------------------|
| 1     | Expanding Window Training              | Understand how training data volume affects model accuracy               |
| 2     | Sliding Window Forecasting             | Evaluate model behavior under real-time forecasting scenarios            |
| 3     | Cross-Site Transfer Learning           | Explore transferring knowledge between data-rich and data-sparse sites  |
| 4     | Global Shared Forecasting Model        | Build a universal model applicable across all stations                   |

## 🧪 Stage 1: Expanding Window Training

### Purpose  
To explore whether increasing the training data length leads to improved forecasting performance at individual stations.

### Setup  
- **Training/Test Split**:  
  Each station’s time series is split into training (first 80%) and test (last 20%).

- **Method**:  
  Use a single-station LSTM model with increasing portions of the training set: 20%, 40%, 60%, 80%.

- **Input**:  
  30 days of CO₂ data → predict the 31st day.

- **Output**:  
  Station-specific MSE at different training sizes.

## ⏳ Stage 2: Sliding Window Forecasting

### Purpose  
To simulate near real-time forecasting by applying a sliding window evaluation across test data.

### Setup  
- **Train/Test Split**:  
  As in Stage 1.

- **Method**:  
  For each test step, slide the input window by 1 day, using fixed model weights.

- **Input**:  
  30-day input sequences from a frozen LSTM.

- **Output**:  
  Rolling forecast accuracy (MSE) over the full test window.

## 🔁 Stage 3: Cross-Site Transfer Learning

### Purpose  
To test whether knowledge learned from one station (source) can help predict CO₂ at another (target) station.

### Setup  
- **Transfer Setting**:  
  Train an LSTM model using all training data from a source station, then test on a different target station.

- **Input**:  
  30-day sequences from source → test on 30-day sequences from target.

- **Output**:  
  Matrix of cross-station forecasting performance.

## 🌐 Stage 4: Global Shared Forecasting Model

### Purpose  
To build a unified forecasting model that generalizes across all stations without requiring station-specific tuning.

### Setup  
- **Data Split**:  
  For each station: first 80% for training, last 20% for testing.

- **Sample Construction**:  
  - Input 1: 30-day sequence of normalized CO₂.
  - Input 2: 7-day rolling mean and std of all stations.
  - Output: Day 31 CO₂ value.

- **Training**:  
  Merge all stations' training samples into a single dataset and train a shared LSTM.

- **Output**:  
  A single model evaluated independently on each station.

