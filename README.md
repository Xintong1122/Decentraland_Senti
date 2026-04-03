# Leveraging Large Language Models for Sentiment Analysis: Multi-Modal Analysis of Decentraland's MANA Token

![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status](https://img.shields.io/badge/status-research-success)

**Supplementary resources, data, and code for the paper**

**Authors**: Xintong Wu¹, Peiting Tsai², Nicholas Yuan², Michael Yu², Greg Sun², and Luyao Zhang³\*  
¹ University of Pennsylvania, USA | ² Microsoft, China | ³ Duke Kunshan University, China  
\* *Corresponding Author: [Luyao Zhang](mailto:lz183@duke.edu), Digital Innovation Research Center and Social Science Division, Duke Kunshan University*

---

## Table of Contents
- [Quick Start](#quick-start)
- [Abstract](#abstract)
- [Methodology](#methodology)
- [Repository Structure](#repository-structure)
- [Data](#data)
  - [Collected Data](#collected-data)
  - [Processed Data](#processed-data)
  - [Data Access](#data-access)
- [Results](#results)
- [Citation](#citation)
- [License](#license)
- [Acknowledgments](#acknowledgments)

---

## Quick Start

```bash
# Clone repository
git clone https://github.com/Xintong1122/Decentraland_Senti.git
cd Decentraland_Senti

# Install dependencies
pip install -r requirements.txt

# Run sentiment analysis on Discord messages
python src/sentiment_analysis.py --input data/raw/discord_messages.csv --output data/processed/

# Train and evaluate LSTM models
python src/price_prediction.py --model multimodal --epochs 100
```

---

## Abstract

Decentraland is a leading decentralized virtual reality platform where users interact, create, and trade using its native token, MANA. This study investigates integrating community sentiment analysis with multi-modal data, including traditional financial indicators, to improve cryptocurrency prediction models.

**Research Questions:**
1. What are the overall sentiment trends and patterns of Decentraland community users on Discord?
2. How does multi-modal data, incorporating sentiment and technical indicators, contribute to predicting token returns?

**Methodology:** We utilize a fine-tuned RoBERTa-based large language model (LLM) from Hugging Face for sentiment analysis of user discussions in the Decentraland community on Discord. We develop two Long Short-Term Memory (LSTM) models: (1) a baseline using only historical price data, and (2) a multi-modal model incorporating sentiment scores, trading volume, market capitalization, and technical indicators.

**Key Findings:**
- Community sentiment within Decentraland is predominantly neutral with a slight positive tendency
- The multi-modal LSTM demonstrates improved prediction accuracy compared to the baseline price-only model
- Sentiment analysis provides complementary signals beyond traditional technical indicators for crypto price forecasting

---

## Methodology

### Sentiment Analysis Pipeline
- **Model**: Fine-tuned RoBERTa-base from Hugging Face (`cardiffnlp/twitter-roberta-base-sentiment`)
- **Classes**: Positive (1), Neutral (0), Negative (-1)
- **Preprocessing**: URL removal, mention normalization, emoji translation, lowercasing
- **Output**: Continuous sentiment scores (-1 to 1) and categorical labels

### Multi-Modal LSTM Architecture
| Component | Baseline LSTM | Multi-Modal LSTM |
|-----------|--------------|------------------|
| **Input Features** | OHLCV prices | OHLCV + Sentiment + Market Cap + Volatility indicators |
| **Sequence Length** | 30 days | 30 days |
| **Architecture** | 2-layer LSTM (128 hidden units) | 2-layer LSTM (128 hidden units) with attention mechanism |
| **Output** | Next-day closing price | Next-day closing price + Direction (up/down) |
| **Metrics** | RMSE, MAE, MAPE | RMSE, MAE, MAPE, Directional Accuracy |

![Pipeline](docs/figures/pipeline_flowchart.png)
*Figure 1: Architecture of the multi-modal sentiment analysis and price prediction pipeline. Discord messages undergo preprocessing and RoBERTa-based sentiment classification, which feeds into an LSTM network alongside traditional financial indicators for MANA token price forecasting.*

---

## Repository Structure

```
├── data/
│   ├── raw/                    # Original Discord exports and price data
│   │   ├── discord/            # Discord CSV exports (not included, see Data Access)
│   │   └── prices/             # MANA historical OHLCV data
│   ├── processed/              # Cleaned and feature-engineered datasets
│   └── dictionary.md           # Detailed data schema documentation
├── models/
│   ├── roberta_sentiment/      # Fine-tuned RoBERTa checkpoints
│   └── lstm/                   # Trained LSTM model weights
├── src/
│   ├── preprocessing/
│   │   ├── discord_cleaner.py  # Discord message preprocessing
│   │   and feature_engineering.py # Technical indicator calculation
│   ├── sentiment_analysis.py   # RoBERTa inference script
│   └── price_prediction.py     # LSTM training and evaluation
├── notebooks/
│   ├── 01_eda.ipynb            # Exploratory data analysis
│   ├── 02_sentiment_tuning.ipynb # Model fine-tuning experiments
│   └── 03_lstm_evaluation.ipynb  # Prediction model analysis
├── results/
│   ├── figures/                # Generated plots and visualizations
│   └── metrics.json            # Model performance metrics
├── requirements.txt
└── README.md
```

---

## Data

### Collected Data

To collect data from Discord, we use **[DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter)**—an application that exports message history from Discord channels to CSV/JSON.

#### Data Dictionary: MANA_token_price

| Variable Name | Unit | Data Type | Description |
|--------------|------|-----------|-------------|
| Date | Days | datetime | The trading date (YYYY-MM-DD) |
| Open | USD | float | Opening price of the asset |
| High | USD | float | Highest price of the asset |
| Low | USD | float | Lowest price of the asset |
| Close | USD | float | Closing price of the asset |
| Volume | Tokens | int | Total number of tokens traded |
| Market_Cap | USD | float | Total market capitalization |

#### Data Dictionary: Discord_messages (Raw)

| Variable Name | Unit | Data Type | Description |
|--------------|------|-----------|-------------|
| AuthorID | Count | int64 | Unique identifier for message authors |
| Author | Name | string | Username of the participant |
| Date_original | Timestamp | datetime | Original timestamp (ISO 8601 format) |
| Date | Days | datetime | Normalized date (YYYY-MM-DD) |
| Content | Text | string | Message text content |
| Attachments | Count | int | Number of attached files/images |
| Reactions | Count | int | Number of emoji reactions |

### Processed Data

#### Data Dictionary: message_with_sentiment

| Variable Name | Unit | Data Type | Description |
|--------------|------|-----------|-------------|
| AuthorID | Count | int64 | Unique user identifier |
| Author | Name | string | Username |
| Date | Days | datetime | Message date |
| Content | Text | string | Cleaned message text |
| RoBERTa_Label | Category | string | Sentiment label (positive/neutral/negative) |
| RoBERTa_Score | Score | float | Normalized sentiment score (-1 to 1) |
| VADER_Label | Category | string | Baseline VADER sentiment label |
| VADER_Score | Score | float | VADER compound score |

#### Data Dictionary: daily_sentiment_aggregated

| Variable Name | Unit | Data Type | Description |
|--------------|------|-----------|-------------|
| Date | Days | datetime | Trading date |
| Avg_RoBERTa_Score | Score | float | Daily average sentiment score |
| Message_Count | Count | int | Number of messages per day |
| Positive_Ratio | Ratio | float | Percentage of positive messages |
| Negative_Ratio | Ratio | float | Percentage of negative messages |

**Notes:**
- All timestamps are in UTC
- Sentiment scores derived using fine-tuned RoBERTa model from Hugging Face
- Missing values in price data handled via forward-fill for weekends/holidays

### Data Access

**Important:** Due to Discord's Terms of Service and privacy considerations, raw message content is **not included** in this repository.

To replicate the dataset:
1. Install [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) (CLI or GUI version)
2. Export Decentraland Discord channels (recommended date range: 2021-2024)
3. Place CSV files in `data/raw/discord/`
4. Run preprocessing: `python src/preprocessing/discord_cleaner.py`

Historical MANA price data is available via [CoinGecko API](https://www.coingecko.com/en/api) or Yahoo Finance.

---

## Results

Our multi-modal LSTM incorporating sentiment analysis outperforms the baseline price-only model:

| Model | RMSE | MAE | Directional Accuracy | R² |
|-------|------|-----|---------------------|-----|
| Baseline (Price only) | 0.089 | 0.067 | 52.3% | 0.41 |
| **Multi-Modal (Price + Sentiment)** | **0.072** | **0.054** | **61.8%** | **0.58** |

*Table 1: Performance comparison on test set (last 3 months of data)*

**Key Visualizations:**
- `results/figures/sentiment_price_correlation.png`: Overlay of sentiment trends vs. MANA price movements
- `results/figures/model_comparison.png`: Predicted vs. actual prices for both models
- `results/figures/confusion_matrix.png`: Sentiment classification accuracy

---

## Citation

If you use this code or data in your research, please cite:

```bibtex
@article{wu2025decentraland,
  title={Leveraging Large Language Models for Sentiment Analysis: Multi-Modal Analysis of Decentraland's MANA Token},
  author={Wu, Xintong and Tsai, Peiting and Yuan, Nicholas and Yu, Michael and Sun, Greg and Zhang, Luyao},
  journal={...},
  year={2025},
  publisher={...}
}
```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Data Usage Notice:** Discord message data is subject to [Discord's Terms of Service](https://discord.com/terms). Cryptocurrency price data is sourced from public APIs. This research is for academic purposes only and does not constitute financial advice.

---

## Acknowledgments

- The Decentraland community for providing open access to Discord discussions
- [Hugging Face](https://huggingface.co/) for the `transformers` library and pre-trained RoBERTa models
- [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) by Tyrrrz for data collection tools

```
