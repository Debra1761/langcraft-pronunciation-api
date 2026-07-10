# Langcraft Pronunciation Assessment API

Build detailed, phoneme-aware speech feedback into your product.

Langcraft Speech API analyzes spoken audio and returns structured data for pronunciation assessment, reading fluency, speech therapy, and linguistic analysis. Send a recording with optional reference text or IPA; receive word and phoneme alignment, timestamps, scoring, detected edits, and optional prosody and proficiency metrics.

## What you can build

- Pronunciation exercises with per-sound feedback
- Reading-fluency assessment
- Speech-therapy and linguistic-analysis workflows
- Automated feedback for spoken language learning

## Features

- **Phoneme-level feedback:** expected and predicted IPA phones, match scores, and grades.
- **Precise timing:** word and phone timestamps in milliseconds.
- **Reference alignment:** use target text or a direct IPA phone sequence; omit both for automatic transcription.
- **Detected edits:** identify substituted, deleted, and inserted phones.
- **Prosody and speaking metrics:** optional pitch/stress contours plus fluency, prosody, intelligibility, pause, and speaking-rate metrics.
- **Multilingual analysis:** supports 40+ languages. For non-English use the documented experimental `aurora-1` model selector (currently recommended for German, French, and Spanish).

## Quick start

Request access and create an API key, then submit a multipart request:

```bash
export LANGCRAFT_API_KEY="your_api_key"

curl -s -X POST https://api.langcraft.world/v1/speech/analyze \
  -H "x-api-key: $LANGCRAFT_API_KEY" \
  -F "audio=@speech.wav" \
  -F "reference_text=I can do it tomorrow." \
  -F "lang=en" \
  -F "dialect=en-us" \
  -F "enable_prosody_contours=true" \
  -F "enable_proficiency_metrics=true"
```

Supported audio formats: WAV, M4A, FLAC, MP3, OGG, and WEBM.

### Choose an analysis mode

| Goal | Send |
| --- | --- |
| Assess a learner reading target text | `audio`, `reference_text`, and `lang` or `dialect` |
| Assess an explicit pronunciation contrast | `audio` and `reference_phones` (a space-separated IPA sequence) |
| Transcribe speech with word timing | `audio` only |

`text` is an alias for `reference_text`; `ipa` is an alias for `reference_phones`.

## Read the response

The API returns one JSON object. This abridged example shows the fields most applications use first:

```json
{
  "language": "en",
  "dialect": "en-us",
  "reference_text": "I can do it tomorrow.",
  "audio": { "duration_sec": 1.85 },
  "summary": {
    "avg_match_score_pct": 96.37,
    "total_phones": 14,
    "num_insertions": 0,
    "num_deletions": 0,
    "num_substitutions": 1
  },
  "word_groups": [
    {
      "text": "cities",
      "summary": { "avg_match_score_pct": 90.44 },
      "phones": [
        {
          "target": "s",
          "predicted": "z",
          "timestamp_ms": { "start": 1860.39, "end": 2060.44 },
          "match_score_pct": 90.48,
          "grade": "substitute"
        }
      ]
    }
  ]
}
```

Use `summary` for an utterance-level overview, `word_groups` for learner-facing word feedback, and each phone entry for sound-level visualization or correction. Enable `enable_prosody_contours` and `enable_proficiency_metrics` when you need those optional outputs.

## Documentation and access

- [Product and access request](https://platform.langcraft.world/)
- [Getting started guide](https://docs.langcraft.world/quickstart)
- [Output reference](https://docs.langcraft.world/output)
- [Supported languages and dialects](https://docs.langcraft.world/dialects)
- [Full API documentation](https://docs.langcraft.world/)

Questions? Contact [hello@langcraft.world](mailto:hello@langcraft.world).

## Notes

This repository provides integration guidance for Langcraft. For production use, always consult the current API documentation for the latest supported languages, request options, access terms, and limits.

