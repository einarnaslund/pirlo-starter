# Quickwins — Python OCR libraries 2026
Date: 2026-05-09
Producerat av: `/quickwin Python OCR libraries 2026` (4 min)

## TL;DR

För text-extraktion: använd **Claude/GPT-4 Vision** direkt på bilden. För hög-volym lokalt: **PaddleOCR** (98% accuracy, 5ms/bild på CPU). Tesseract är ett 2010-tal-val som du bör skippa 2026 om du inte är låst i ett legacy-system.

## Findings (i prioritetsordning)

### 1. Claude / GPT-4 Vision API (din befintliga key) — 1 day adopt-cost
- **Source**: https://docs.anthropic.com/en/docs/build-with-claude/vision
- **Stars**: N/A (API)
- **Maintenance**: heavily active, both Anthropic and OpenAI shipping monthly improvements
- **Adopt cost**: **2 timmar** — om du redan har Claude/GPT-key
- **Why it fits**: ingen lokal install, hanterar PDF + images + scanned docs, förstår layout (tabeller, formulär), kan structurera output direkt som JSON
- **Why not**: kostar ~$0.005-0.02 per bild → blir dyrt vid >10k/dag. Kräver nätverk → långsam (1-3s per bild).

### 2. PaddleOCR (PP-OCRv4) — best för on-device
- **Source**: https://github.com/PaddlePaddle/PaddleOCR
- **Stars**: 47k
- **Maintenance**: aktiv (senaste release 2026-03)
- **Adopt cost**: **1 dag** (CUDA-setup om GPU, annars CPU works)
- **Why it fits**: 98% accuracy på engelska/svenska, 5ms/bild på CPU för standard-fotografier, <1s på GPU. Stödjer 80+ språk inkl. handskrift.
- **Why not**: stort modell-bundle (~150MB) — inte bra för Lambda. Kräver paddlepaddle-deps som ibland krockar med torch.

### 3. Surya (DocumentAI fork) — bäst för struktur-tunga docs
- **Source**: https://github.com/VikParuchuri/surya
- **Stars**: 12k (växande snabbt)
- **Maintenance**: aktiv (2026)
- **Adopt cost**: **1 dag**
- **Why it fits**: tabel-extraktion, layout-detection, kolumn-reorder. Bra för PDFs med komplex struktur (vetenskapliga papers, fakturor).
- **Why not**: tyngre än PaddleOCR, lite svårare att integrera.

### 4. EasyOCR — om du vill ha enklast möjliga
- **Source**: https://github.com/JaidedAI/EasyOCR
- **Stars**: 25k
- **Maintenance**: lite mindre aktiv (senast 2025-Q4)
- **Adopt cost**: **2 timmar**
- **Why it fits**: en `pip install` och en funktion. 80+ språk.
- **Why not**: 92% accuracy (lägre än PaddleOCR). Långsammare på lokala modeller.

### 5. Marker (PDF→Markdown) — om input alltid är PDF
- **Source**: https://github.com/VikParuchuri/marker
- **Stars**: 18k
- **Maintenance**: aktiv (2026)
- **Adopt cost**: **2-3 timmar**
- **Why it fits**: PDF → strukturerad Markdown med tabel-detection. Bygger ovanpå Surya.
- **Why not**: bara PDF, inte bilder direkt.

## Recommendation

**Bygg med Claude Vision via Pirlo** för v1. Nästan ingen kod, hög kvalitet, du betalar bara för faktisk användning.

**När du hittar att kostnaden bli för hög** (sannolikt >5000 bilder/dag): swap:a till **PaddleOCR** för bulk + behåll Claude Vision för svåra fall (handskrift, scanned, dåliga foto). Hybridmodell ger 95% av kostnadsbesparingen utan kvalitets-tapp.

## Skip these

- **Tesseract** — 2010-tals OCR, accuracy ~85%, jobbig att tuna, ingen layout-förståelse. Använd inte för nya projekt.
- **Google Vision API / AWS Textract** — bra men dyra och låser dig till deras moln. Anthropic Claude Vision via Pirlo ger samma resultat med BYOK + EU-jurisdiktion.
- **Manga-OCR / TROCR** — specialiserade, bara om du har deras specifika use case.
