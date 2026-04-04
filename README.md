# Leveraging Large Language Models for Sentiment Analysis: Multi-Modal Analysis of Decentraland's MANA Token

![Python](https://img.shields.io/badge/python-3.9+-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status](https://img.shields.io/badge/status-research-success)

Supplementary resources, data, and code for the paper

## Table of Contents

- [Quick Start](#quick-start)
- [Abstract](#abstract)
- [Methodology](#methodology)
- [Repository Structure](#repository-structure)
- [Data](#data)
  - [Collected Data](#collected-data)
  - [Processed Data](#processed-data)
- [Code](#code)
- [Spotlight Visualizations](#spotlight-visualizations)
- [Citation](#citation)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Quick Start

```bash
# Clone repository
git clone https://github.com/Xintong1122/Decentraland_Senti.git
cd Decentraland_Senti

# Explore the data files in the Data/ folder
# Open and run the Jupyter notebook in Code/Price_Prediction.ipynb
```

## Abstract

Decentraland is a leading decentralized virtual reality platform where users interact, create, and trade using its native token, MANA. This study investigates integrating community sentiment analysis with multi-modal data, including traditional financial indicators, to improve cryptocurrency prediction models.

**Research Questions:**
- What are the overall sentiment trends and patterns of Decentraland community users on Discord?
- How does multi-modal data, incorporating sentiment and technical indicators, contribute to predicting token returns?

**Methodology:**
We utilize a fine-tuned RoBERTa-based large language model (LLM) from Hugging Face for sentiment analysis of user discussions in the Decentraland community on Discord. We develop two Long Short-Term Memory (LSTM) models: (1) a baseline using only historical price data, and (2) a multi-modal model incorporating sentiment scores, trading volume, market capitalization, and technical indicators.

**Key Findings:**
- Community sentiment within Decentraland is predominantly neutral with a slight positive tendency
- The multi-modal LSTM demonstrates improved prediction accuracy compared to the baseline price-only model
- Sentiment analysis provides complementary signals beyond traditional technical indicators for crypto price forecasting

## Methodology

### Sentiment Analysis Pipeline

| Aspect | Details |
|--------|---------|
| **Model** | Fine-tuned RoBERTa-base from Hugging Face (`cardiffnlp/twitter-roberta-base-sentiment`) |
| **Classes** | Positive (1), Neutral (0), Negative (-1) |
| **Preprocessing** | URL removal, mention normalization, emoji translation, lowercasing |
| **Output** | Continuous sentiment scores (-1 to 1) and categorical labels |

### Multi-Modal LSTM Architecture

| Component | Baseline LSTM | Multi-Modal LSTM |
|-----------|---------------|------------------|
| Input Features | OHLCV prices | OHLCV + Sentiment + Market Cap + Volatility indicators |
| Sequence Length | 30 days | 30 days |
| Architecture | 2-layer LSTM (128 hidden units) | 2-layer LSTM (128 hidden units) with attention mechanism |
| Output | Next-day closing price | Next-day closing price + Direction (up/down) |
| Metrics | RMSE, MAE, MAPE | RMSE, MAE, MAPE, Directional Accuracy |

## Repository Structure

```
Decentraland_Senti/
├── Code/
│   └── Sentiment_analysis.ipynb      # Jupyter notebook for sentiment analysis using LLM
│   └── Price_prediction.ipynb    # Jupyter notebook for LSTM price prediction models
├── Data/                          # Dataset files (CSV format)
│   ├── Decentraland_general.csv   # General Decentraland community data
│   ├── MANA_price.csv             # MANA token historical OHLCV price data
│   ├── MANA_typical_price.csv     # MANA typical price calculations
│   ├── daily_processed_scores.csv # Daily processed sentiment scores
│   ├── daily_sentiment_scores.csv # Daily aggregated sentiment scores
│   ├── filtered_message.csv       # Filtered Discord messages
│   ├── message_cleaned.csv        # Cleaned Discord message data
│   └── message_with_sentiment.csv # Discord messages with sentiment labels
├── sportlight/                    # Visualization images and spotlight assets
│   ├── n0.png                     # Visualization
│   ├── n1.png                     # Visualization
│   ├── pp1.png                    # Price prediction visualization
│   ├── sa1.png                    # Sentiment analysis visualization
│   ├── sa11.png                   # Sentiment analysis visualization
│   └── sa2.png                    # Sentiment analysis visualization
|   └── new.png                    # Sentiment analysis and price prediction visualization (combined)
└── README.md                      # This file
```

## Data

### Collected Data

To collect data from Discord, we use [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) — an application that exports message history from Discord channels to CSV/JSON.

#### Data Dictionary: MANA_price.csv

Historical OHLCV data for MANA (ERC-20 utility token) spanning June 15, 2023 – June 14, 2024 (366 days) obtained from [CoinMarketCap](https://coinmarketcap.com/).

| Variable Name | Unit | Data Type | Description |
|---------------|------|-----------|-------------|
| Date | Days | datetime | The trading date (YYYY-MM-DD) |
| Open | USD | float | Opening price of the asset |
| High | USD | float | Highest price of the asset |
| Low | USD | float | Lowest price of the asset |
| Close | USD | float | Closing price of the asset |
| Volume | Tokens | int | Total number of tokens traded |
| Market_Cap | USD | float | Total market capitalization |
#### Data Dictionary: Discord Messages (Raw)

| Variable Name | Unit | Data Type | Description |
|---------------|------|-----------|-------------|
| AuthorID | Count | int64 | Unique identifier for message authors |
| Author | Name | string | Username of the participant |
| Date_original | Timestamp | datetime | Original timestamp (ISO 8601 format) |
| Date | Days | datetime | Normalized date (YYYY-MM-DD) |
| Content | Text | string | Message text content |
| Attachments | Count | int | Number of attached files/images |
| Reactions | Count | int | Number of emoji reactions |

### Processed Data

#### Data Dictionary: message_with_sentiment.csv

| Variable Name | Unit | Data Type | Description |
|---------------|------|-----------|-------------|
| AuthorID | Count | int64 | Unique user identifier |
| Author | Name | string | Username |
| Date | Days | datetime | Message date |
| Content | Text | string | Cleaned message text |
| RoBERTa_Label | Category | string | Sentiment label (positive/neutral/negative) |
| RoBERTa_Score | Score | float | Normalized sentiment score (-1 to 1) |
| VADER_Label | Category | string | Baseline VADER sentiment label |
| VADER_Score | Score | float | VADER compound score |

#### Data Dictionary: daily_sentiment_scores.csv

| Variable Name | Unit | Data Type | Description |
|---------------|------|-----------|-------------|
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

**Important:** Due to Discord's Terms of Service and privacy considerations, raw message content is included in this repository in cleaned and processed forms only.

To replicate the dataset:
1. Install [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) (CLI or GUI version)
2. Export Decentraland Discord channels (recommended date range: 2021-2024)
3. Run preprocessing using the methodology described in the paper

Historical MANA price data is available via [CoinGecko API](https://www.coingecko.com/en/api) or Yahoo Finance.

## Code

The `Code/` directory contains:

- **Price_prediction.ipynb** - Jupyter notebook implementing:
  - Data preprocessing and feature engineering
  - Baseline LSTM model (price-only)
  - Multi-modal LSTM model (price + sentiment)
  - Model evaluation and comparison
  - Token-return
  - Visualization of results
- **Sentiment_analysis.ipynb** - Jupyter notebook implementing:
  - Data preprocessing and feature engineering
  - Sentiment score and sentiment label
  - Visualization of results

## Spotlight Visualizations

The `sportlight/` directory contains key visualizations from the research:

- **n0.png, n1.png** - Neutral sentiment analysis visualizations
- **pp1.png** - Price prediction model results
- **sa1.png, sa11.png, sa2.png** - Sentiment analysis visualizations
- **new.png** - Sentiment analysis and price prediction model results (combined)

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

## License

This project is licensed under the MIT License.

**Data Usage Notice:** Discord message data is subject to [Discord's Terms of Service](https://discord.com/terms). Cryptocurrency price data is sourced from public APIs. This research is for academic purposes only and does not constitute financial advice.

## Acknowledgments

- The Decentraland community for providing open access to Discord discussions
- [Hugging Face](https://huggingface.co/) for the [transformers](https://github.com/huggingface/transformers) library and pre-trained RoBERTa models
- [DiscordChatExporter](https://github.com/Tyrrrz/DiscordChatExporter) by Tyrrrz for data collection tools
- Historical MANA price data is available via [CoinMarketCap](https://coinmarketcap.com/), [CoinGecko API](https://www.coingecko.com/en/api), or Yahoo Finance.
