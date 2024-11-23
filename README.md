# 

# SentiGPT: Analyzing User Sentiment in Decentraland using Large Language Models

## *Supplementary resources, data, and code*
by **Xintong Wu**, **Peiting Tsai**, **Nicholas Yuan** , **Michael Yu**, **Greg Sun** and **Luyao Zhang***
(* *the corresponding author: lz183@duke.edu*)

<img src="https://github.com/Xintong1122/Decentraland_LLM/blob/main/Figures/Flowchart%20for%20sentiment%20analysis%20using%20LLM.png" width="600" alt="article-overview" /><br/>

*Figure 1. Flowchart for sentiment analysis using LLM.*

## Table of Contents
- [Data](https://github.com/Xintong1122/Decentraland_LLM/tree/main#data)
- [Code]()
- [Images]()
- [Reference]()

## Data

### Collected Data

For collecting data from Discord, please refer to [**DiscordChatExporter**](https://github.com/Tyrrrz/DiscordChatExporter). DiscordChatExporter is an application that can be used to export message history from any Discord channel to a file.

#### Meta Data Infomation

| Data Files  | Data Type | Data Content |
| ------------- | ------------- | ------------- |
| [Discord_message.csv](https://github.com/Xintong1122/Decentraland_LLM/blob/main/Data/Discord_message.csv)  | Queried  | original Discord User Messages |

#### Data Dictionary

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

#### Meta Data Infomation

| Data Files  | Data Type | Data Content |
| ------------- | ------------- | ------------- |
| [message_cleaned.csv](https://github.com/Xintong1122/Decentraland_LLM/blob/main/Data/message_cleaned.csv)  | Processed | cleaned Discord User Message |
| [message_with_sentiment.csv](https://github.com/Xintong1122/Decentraland_LLM/blob/main/Data/message_with_sentiment.csv)  | Processed | Discord User Message with sentiment label and sentiment score |
| [daily_sentiment_scores.csv](https://github.com/Xintong1122/Decentraland_LLM/blob/main/Data/daily_sentiment_scores.csv)  | Processed | Discord User Message with daily sentiment label and sentiment score  |

#### Data Dictionary

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


## Code

| **Code Type** | **Google Colab File Name**|
| ------- | ------- |
| Process | [Processing_data.ipynb](https://github.com/Xintong1122/Decentraland_LLM/blob/main/Code/Processing_data.ipynb) |
| Analysis | [Analyzing_data.ipynb](https://github.com/Xintong1122/Decentraland_LLM/blob/main/Code/Analyzing_data.ipynb) |

