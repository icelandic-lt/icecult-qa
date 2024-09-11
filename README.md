# Icelandic Culture and History QA Dataset

A question answering dataset intended to measure a large language model's knowledge of Icelandic facts on culture and history and its ability to answer questions correctly. 

The dataset is split into two parts, a `gold` corpus and a `silver` corpus. The gold corpus consists of 2,000 pairs of manually reviewed questions and answers while the silver corpus consists of 10,644 pairs of questions and answers which have not been manually reviewed. All pairs were originally automatically created by GPT-4-turbo based on Icelandic [Wikipedia articles](https://huggingface.co/datasets/wikimedia/wikipedia) and online news from RÚV, which are included in the [Icelandic Gigaword Corpus](http://hdl.handle.net/20.500.12537/236).

In the gold corpus, 1,900 pairs are from Wikipedia articles while 100 pairs are from news texts. In the silver corpus, 9,610 pairs are from Wikipedia articles and 1,034 pairs are from news texts.

## Format

The gold and silver corpora are published as JSONL files, where each question and answer pair is a JSON object in the following format:

```
{
    "query": "",
    "answer": "",
    "question_id": "A uuid for the question and answer pair",
    "question_score": "This is not included in the gold subset, but is otherwise a score given by GPT-4-turbo",
    "document_score": "A score given by GPT-4-turbo",
    "url": "The original URL of a Wikipedia or news article",
    "xml_id": "This is only included in the news data, and references the XML ID of an original Icelandic Gigaword Corpus XML file containing the article",
    "title": "The title of the original article",
    "context": "The original article"
}
```

The gold corpus is additionally published in formats compatible with [BIG-bench](https://github.com/google/BIG-bench) and [OpenAI-evals](https://github.com/openai/evals). Both formats consist of JSON objects.

The BIG-bench format is the following:
```
{
    "input": the question as a string object,
    "target": the answer as a string object
}
```

The OpenAI-evals format is the following:
```
{
    "input": a list consisting of two dicts, one for the system prompt and one for the question. The system prompt is in all cases "Þú ert vandvirk aðstoðarmanneskja. Svaraðu eftirfarandi spurningu með hnitmiðuðu svari.", which translates to "You are a helpful assistant. Answer the following question with a concise answer.".
    "ideal": the answer
}
```

## Evaluation 

An example method of using the dataset to evaluate language models is the method used in the [Icelandic LLM leaderboard](https://huggingface.co/spaces/mideind/icelandic-llm-leaderboard). A GPT model is used as an evaluator to compare a generated answer to the original answer for semantic similarity and rate the answer on a scale of (0, "poor"), (0.5, "fair") and (1, "excellent").

BIG-bench and OpenAI-evals are limited to certain available evaluation metrics. Within BIG-bench, a ROUGE-L score is returned when using the gold dataset to evaluate models and within OpenAI-evals, fuzzy match is used.