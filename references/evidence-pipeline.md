# Evidence-first recipe reconstruction

## Evidence hierarchy

Prefer evidence in this order when conflicts occur:

1. creator's explicit correction or ingredient table;
2. video/on-image ingredient overlay;
3. creator's spoken instruction/subtitle;
4. creator caption;
5. clearly visible action or ingredient;
6. user-confirmed correction;
7. third-party comment summary only as a lead to verify.

Record disagreements rather than silently merging them.

## Detail pass

A coarse frame sample often misses values shown for one second. Run a denser second pass around cooking sections and search specifically for:

- grams, ml, counts;
- ratios;
- oven/air-fryer temperature and time;
- oil temperature stages;
- heat level;
- rest, soak, steam, simmer, pressure-cook and refrigeration times;
- visual doneness cues;
- sauce components and garnish;
- paired staple, drink, side dish, or final plating.

Cross-check contact sheets against transcript and metadata. Keep the full timeline so breakfast drinks, side dishes, rice, toast, eggs, sauces, and plating are not accidentally omitted.

## Unknown-field language

Use clear uncertainty labels:

- `原视频未标注克数`
- `视频未给数量`
- `画面可见，名称待确认`
- `作者口径，未独立复算`
- `家庭版建议量（非作者原配方）`

Never convert an unnamed quantity into a standard serving. Never let the image model manufacture numbers.

## Multi-dish media

Identify the complete dish list before generating anything. Produce one structured recipe and one card per confirmed dish. If a dish is visible but its method is absent, label it incomplete or omit it from the final cooking card.

## Executable step standard

Each step should state, when evidence supports it:

- ingredient preparation and cut shape;
- sequence and placement;
- hand action such as fold, press, seal, stir, coat, rotate, or return to pan;
- heat/time/temperature;
- observable finish cue;
- short failure-prevention note.

Do not add generic steps such as blanching, thickening, cooling, or marinating unless shown/spoken or explicitly labeled as an optional adaptation.
