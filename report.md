# Report: Inferring Sentiment and Topics

## Approach
I designed three prompt versions for the same Lumina lamp review and fictitious government survey article. Holding the source texts constant allows a useful comparison of instruction styles. Version 1 uses a creative shopping-columnist and news-editor role with open-ended questions. Version 2 explicitly requests sentiment, emotions, anger, product, brand, article topics, and topic flags. Version 3 adds a JSON schema, a supporting quotation, and rules against unsupported assumptions. Each version uses the notebook's completion helper at temperature zero.

## Findings from the source texts
The review's overall sentiment is positive. The reviewer praises the company's fast, helpful response despite a broken string and missing part. The product is a lamp and the brand is Lumina. Plausible emotions include satisfaction, gratitude, and enthusiasm; these are interpretive labels rather than a unique correct answer. There is no clear expression of anger. Delivery problems alone do not establish that the reviewer is angry.

The article covers NASA, employee satisfaction, and federal government. Local government and engineering are not discussed. NASA's association with engineering does not make engineering a topic of this particular article. Five reasonable short topics are NASA, government survey, employee satisfaction, agency management, and workplace improvement. The article explicitly identifies itself in the lab as fictitious, so its names and percentages should not be presented as verified news.

## Comparison and failure risks
The open-ended version may provide a readable summary while omitting fields needed by a program. The specific version makes coverage easier to assess, but does not enforce a single output format. The evidence-based version is designed to make automated checking easier and reduce unsupported inferences. These are design expectations, not measured performance differences: no live API results were obtained because an API key was unavailable. I cannot claim that any version actually hallucinated or that version 3 performed best.

One concrete problem was identifiable without calling the model: the original alert code split a dictionary-like response on commas and colons. Braces and quoted keys could cause conversion errors or prevent lookup of the nasa key. I replaced this with a request for valid JSON, json.loads parsing, and checks for the expected keys and integer flags. JSON parsing can still fail if the model ignores the formatting instruction; that failure should remain visible rather than be silently accepted.

## What I learned
Inferring requires separating what a text says from background associations. Overall sentiment can differ from the tone of individual events, and emotions allow more interpretation than explicit product names. Clear output contracts support reliable downstream code, while evidence requirements make results easier to audit. The notebook includes three runnable experiments and reference assertions for the structured version. Once credentials are configured, the saved outputs can support an empirical comparison; the current report distinguishes textual findings from predictions about model behavior.
