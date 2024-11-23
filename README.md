# Leveraging Large Language Models for Sentiment Analysis: Multi-Modal Analysis of Decentraland's MANA Token

## *Supplementary resources, data, and code*
by **Xintong Wu**, **Peiting Tsai**, **Nicholas Yuan** , **Michael Yu**, **Greg Sun** and **Luyao Zhang***
(* *the corresponding author: lz183@duke.edu*)

<img src="https://github.com/Xintong1122/Decentraland_LLM/blob/main/Figures/Flowchart%20for%20sentiment%20analysis%20using%20LLM.png" width="600" alt="article-overview" /><br/>

*Figure 1. Flowchart for sentiment analysis using LLM.*

## Abstract
Decentraland is a leading decentralized virtual reality platform where users interact, create, and trade using its native token, MANA. The study investigates integrating community sentiment analysis with multi-modal data, including traditional financial indicators, to improve cryptocurrency prediction models. We address two main research questions: What are the overall sentiment trends and patterns of Decentraland community users on Discord? How does multi-modal data, incorporating sentiment and technical indicators, contribute to predicting token returns? We utilized a BERT-based large language model (LLM) from Hugging Face for sentiment analysis of user discussions in the Decentraland community on Discord. We developed two Long Short-Term Memory (LSTM) models, one using only historical price data and another incorporating multi-modal data, including sentiment scores, trading volume, market capitalization, and other relevant indicators. Our findings indicate that community sentiment within Decentraland is predominantly neutral with a positive tendency. The model incorporated multi-modal data, including sentiment analysis, and demonstrated improved prediction accuracy compared to the model that used only historical prices. This study substantially contributes to the field by demonstrating the transformative potential of multi-modal data integration in financial forecasting. Providing a more comprehensive and accurate prediction model sets a new benchmark for future research at the intersection of cryptocurrency, sentiment analysis, and the Metaverse, paving the way for more informed investment strategies and market analyses.

## Data

### Collected Data

To collect data from Discord, please refer to [**DiscordChatExporter**](https://github.com/Tyrrrz/DiscordChatExporter). DiscordChatExporter is an application that can be used to export message history from any Discord channel to a file.

#### Data Dictionary

- **MANA_token_price**
| **Variable Name** | **Unit**    | **Data Type** | **Description**                                              |
|--------------------|-------------|---------------|--------------------------------------------------------------|
| Date          | DAays           | int64          | The trading date.                                            |
| Open          | USD         | Float         | Opening price of the asset on the trading day.              |
| High          | USD         | Float         | Highest price of the asset on the trading day.              |
| Low           | USD         | Float         | Lowest price of the asset on the trading day.               |
| Close         | USD         | Float         | Closing price of the asset on the trading day.              |
| Volume        | Tokens      | Integer       | Total number of tokens traded during the trading day.       |
| Market Cap    | USD         | Float         | Total market capitalization of the asset on the trading day. |


- **Discord_message**

| **Variable Name**	| **Unit**	| **Data Type**	| **Description** |
| ------- | ------- | ------- | ------- |
| AuthorID	| Count | int64 | This identifier uniquely distinguishes the authors of the discussions, allowing for tracking and attribution. |
| Author | Name |int64 | The name or username of the discussion participants. |
| Date_original | Minutes | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in minutes). |
| Date | Days | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in days). |
| Content | Text | int64 | The textual content of the discussions, including messages, comments, and replies. |
| Attachments | File/Link/Image | int64 | Information regarding any attached files, images, or media shared within the discussions. |
| Reactions | Emoji | int64 | A record of reactions, such as emojis, associated with each discussion, offering insights into community engagement and sentiment. |


### Processed Data

#### Data Dictionary

- **MANA_typical_price**
| **Variable Name** | **Unit**    | **Data Type** | **Description**                                             |
|--------------------|-------------|---------------|------------------------------------------------------------|
| Date          | Days          | int64          | The trading date.                                            |
| Typical Price | USD           | Float          | Typical price of the asset on the trading day.               |

- **message_cleaned**

| **Variable Name**	| **Unit**	| **Data Type**	| **Description** |
| ------- | ------- | ------- | ------- |
| AuthorID	| Count | int64 | This identifier uniquely distinguishes the authors of the discussions, allowing for tracking and attribution. |
| Author | Name |int64 | The name or username of the discussion participants. |
| Date_original | Minutes | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in minutes). |
| Date | Days | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in days). |
| Content | Text | int64 | The textual content of the discussions, including messages, comments, and replies. |
| Attachments | File/Link/Image | int64 | Information regarding any attached files, images, or media shared within the discussions. |
| Reactions | Emoji | int64 | A record of reactions, such as emojis, associated with each discussion, offering insights into community engagement and sentiment. |

- **message_with_sentiment**

| **Variable Name**	| **Unit**	| **Data Type**	| **Description** |
| ------- | ------- | ------- | ------- |
| AuthorID	| Count | int64 | This identifier uniquely distinguishes the authors of the discussions, allowing for tracking and attribution. |
| Author | Name |int64 | The name or username of the discussion participants. |
| Date_original | Minutes | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in minutes). |
| Date | Days | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in days). |
| Content | Text | int64 | The textual content of the discussions, including messages, comments, and replies. |
| Attachments | File/Link/Image | int64 | Information regarding any attached files, images, or media shared within the discussions. |
| Reactions | Emoji | int64 | A record of reactions, such as emojis, associated with each discussion, offering insights into community engagement and sentiment. |
|LLM1_Label	| Text | int64 | Sentiment labels derived from respective LLM analyses|
|LLM1_Score	| Count | float | Sentiment scores derived from respective LLM analyses|
|LLM2_Label	| Text | int64 | Sentiment labels derived from respective LLM analyses|
|LLM2_Score	| Count | float | Sentiment scores derived from respective LLM analyses|
|LLM3_Label	| Text | int64 | Sentiment labels derived from respective LLM analyses|
|LLM3_Score | Count | float | Sentiment scores derived from respective LLM analyses|


- **message_with_sentiment**

| **Variable Name**	| **Unit**	| **Data Type**	| **Description** |
| ------- | ------- | ------- | ------- |
| Date | Days | int64 | The timestamp indicates when each discussion occurred, providing a temporal dimension to the dataset (shown in days). |
|LLM1_Sentiment_Score	| Count | float | Sentiment scores derived from respective LLM analyses|
|LLM1_Label	| Text | int64 | Sentiment labels derived from respective LLM analyses|
|LLM2_Sentiment_Score	| Count | float | Sentiment scores derived from respective LLM analyses|
|LLM2_Label	| Text | int64 | Sentiment labels derived from respective LLM analyses|
|LLM3_Sentiment_Score | Count | float | Sentiment scores derived from respective LLM analyses|
|LLM3_Label	| Text | int64 | Sentiment labels derived from respective LLM analyses|

#### Notes:
- All timestamps are in UTC.
- Sentiment scores are derived using a fine-tuned RoBERTa model from Hugging Face.

## Code

| **File Name** | **Description**|
| ------- | ------- |
|Price_Prediction | Sentiment Analysis of Discord Decentraland user message and MANA price prediction |


