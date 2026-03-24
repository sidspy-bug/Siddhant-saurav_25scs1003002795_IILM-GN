# NAVIILM – Campus Assistance (Text, Voice, and Bilingual)

NAVIILM is a campus navigation and information assistant built in Python/Gradio. It indexes a campus dataset, performs fuzzy and semantic search, offers routing when coordinates are available, and can answer in both English and Hindi with optional text-to-speech.

## Features
- **Data indexing**: Loads a campus JSON dataset (supports top-level `CampusData`) and builds searchable entries with optional coordinates.
- **Search modes**: RapidFuzz-based fuzzy search and sentence-transformers semantic search (`all-MiniLM-L6-v2`).
- **Routing**: NetworkX graph with A* pathfinding when coordinate pairs are present in the dataset.
- **Voice output**: gTTS-powered audio; concise/detailed modes and a bilingual (Hindi + English) responder that can mix mic input or typed queries.
- **Recommendations**: Suggests related places alongside the top result.
- **Diagnostics**: Built-in diagnostic cell checks dataset loading, search functions, routing graph, and TTS helpers.

## Requirements
- Python 3.9+ (tested in Google Colab; works locally with the same dependencies)
- ffmpeg installed and on PATH (required for `pydub` when generating audio)
- Python packages:
  ```bash
  pip install gradio rapidfuzz gTTS pydub sentence-transformers scikit-learn networkx
  ```
  The sentence-transformers model downloads weights on first run.

## Dataset
- Provide a campus dataset as `bigdata.json` (default path: `/content/bigdata.json`).
- The loader accepts either a top-level object or an object with a `CampusData` key. Blocks should contain room/place lists under keys such as `rooms`, `places`, `locations`, `items`, or `rooms_list`. Include `coordinates: [x, y]` to enable routing.
- In Colab, you can upload the file when prompted; locally, place it at the path set in `DATA_PATH` near the top of `NAVIILM`.

## Running in Google Colab (recommended)
1. Upload `NAVIILM` to a Colab notebook or open it directly in Colab.
2. Upload `bigdata.json` when prompted (or adjust `DATA_PATH`).
3. Run the cells in order to build the index, embeddings, and graph.
4. Launch a UI:
   - Improved voice UI: `improved_demo.launch(share=True)`
   - Bilingual UI with mic/text: `bilingual_demo.launch(share=True)`
5. Use the “NAVIILM Diagnostic” cell if you need to verify dataset loading, search functions, routing graph, and TTS setup.

## Running locally
1. Ensure ffmpeg is installed and the dependencies above are installed in your environment.
2. Place your dataset at the path specified by `DATA_PATH` (default `/content/bigdata.json`) or change that constant near the top of `NAVIILM`.
3. Execute the `NAVIILM` script in a Jupyter/Colab-like environment (the file includes Colab upload helpers). For a plain Python run, comment out the Colab upload lines and ensure the dataset path exists.
4. After running the setup cells, call one of the launch commands shown above to start the Gradio interface.

## Notes
- The project currently lives as a single notebook-style script (`NAVIILM`).
- If you add coordinates to more places, routing results improve.
- Clean up temporary TTS files in `/tmp` periodically if you generate many responses.
