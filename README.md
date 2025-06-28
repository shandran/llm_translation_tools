# LLM Language Translation Tools
Language translation tools using Large Language Models.

## Evalutor-Optimizer tool
For source documents formatted or converted into markdown. Based on the approach outlined in Ed Donner's *Agentic AI Engineering Course* on Udemy.

If you wish to use source files not already in markdown format, you can convert Word documents into md using the docx python library (this requires some tedious wrangling and parsing of the Word styles into markdown elements).

### George MacDonald public-domain literary classic
George MacDonald's *The Princess and the Goblin* (1872) is a children's literary classic (a work that inspired later fantasy authors J.R.R. Tolkien and C.S. Lewis) in the public domain, and is used to demonstrate the LLM language translation tool.

See [`literary_gemini_translator_openai_evaluator_md_json.ipynb`](https://github.com/shandran/llm_translation_tools/blob/main/literary_classics_translator/George_MacDonald_The_Princess_and_the_Goblin/literary_gemini_translator_openai_evaluator_md_json.ipynb) notebook in the [`George_MacDonald_The_Princess_and_the_Goblin`](https://github.com/shandran/llm_translation_tools/tree/main/literary_classics_translator/George_MacDonald_The_Princess_and_the_Goblin) folder for the source code.

Markdown of [***The Princess and the Goblin***](https://github.com/mlschmitt/classic-books-markdown/blob/main/George%20MacDonald/The%20Princess%20and%20the%20Goblin.md) is available from [**Michael Schmitt**](https://github.com/mlschmitt)'s [**classic-books-markdown**](https://github.com/mlschmitt/classic-books-markdown) repo on GitHub.

### Workflow

1. **Setup**: loading dependencies, api keys, and models (Gemini, OpenAI, Anthropic), setting languages.
2. **Load and chunk source text**: In this example, just the first 50 lines of the markdown are used. The chunking code uses a hybrid approach of setting a target number of tokens (300 works well) while also not breaking up markdown elements. This results in somewhat disorienting blocks as you read it in interlinear format (see below), but it does provide a good context window that doesn't create fragmented breaks. The chunks are formatted into JSON structure and saved in-memory as a list of dictionaries python object. As the chunks are processed, the results and any errors or feedback that are generated are also captured.
3. **Translation function**: in this model, Gemini 2.5 (Flash or Pro) serves as the primary translator. The translation results are stored in 'check' and and errors are also logged.
4. **Evaluator function**: OpenAI (o3 or gpt-4o) serves as the evaluator that checks for accuracy of the translation, logs any feedback, and determines if the 'check' translation passes or not. If it passes, the translation is logged to 'final' along with the model name. If it doesn't pass, an alternative translator (in this case, Claude-4 Sonnet) attempts the translation. An earlier version sent it back the translator function but had a high failure rate, so the alternative translator approach was attempted instead.
5. **Reporting**: generates pass-fail rates, and a token size histogram.
6. **Clean up double-fails**: for the rare instances that "double-fail" in the evaluation step, this cleans up any remaining fails.
7. **Export to json and markdown**: the complete JSON structure is saved, along with reconstructing an 'interlinear' markdown document (the original language first then the translation, alternating back and forth for each chunk), and finally, a translation-only markdown file is saved. (If you wish, markdown documents can be saved back to Word format using the pypandoc python library.)
