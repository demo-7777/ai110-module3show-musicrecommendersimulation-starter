# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

Replace this paragraph with your own summary of what your version does.

---

## How The System Works

Real platforms like Spotify and TikTok mostly blend two ideas. **Collaborative filtering** looks at other users: "people who liked what you liked also played this." **Content-based filtering** looks at the song itself: its genre, tempo, mood, and energy, and finds more songs with similar attributes. At scale they lean heavily on collaborative signals (likes, skips, replays, playlist adds) because they have millions of users to compare.

This version is a small **content-based** recommender. There are no other users — instead the listener is described by a fixed **taste profile**, and every song in the catalog is compared against that profile. Each song earns a compatibility score, and the songs are ranked highest-first to produce the top recommendations. My version prioritizes **genre** and **mood** matches, plus how close a song's **energy** is to the user's target.

**Features each `Song` uses:** genre, mood, energy (0.0–1.0). (The catalog also carries tempo_bpm, valence, danceability, and acousticness, available for later extensions.)

**What the `UserProfile` stores:** favorite_genre, favorite_mood, target_energy (and likes_acoustic for later use).

**How a score is computed (Scoring Rule — one song):** points for a genre match, fewer points for a mood match, plus a similarity score rewarding songs whose energy is close to the target.

**How songs are chosen (Ranking Rule — the whole list):** score every song, sort highest→lowest, return the top *k*.

### Algorithm Recipe (finalized)

| Component | Points | Rule |
|---|---|---|
| Genre match | +2.0 | song genre equals the profile's favorite genre |
| Mood match | +1.0 | song mood equals the profile's favorite mood |
| Energy similarity | 0.0 – 1.0 | `1 - abs(song energy - target energy)`; closer to target scores higher |

Maximum score = **4.0**. Each awarded component also records a short reason (e.g. `"genre match (+2.0)"`) so a recommendation can be explained. Genre outweighs mood because genre is the coarser "what kind of music" signal and mood refines within it; energy is capped at 1.0 so it acts as a tie-breaker but never overrides a genre match. Ranking then sorts all songs by total score and returns the top *k*.

### Expected Biases

Because genre carries the most weight and the catalog leans rock-heavy (5 of 20 songs are rock / hard rock / metal), a rock-leaning profile will tend to dominate the top slots — a small "filter bubble" effect. The system also only rewards *exact* genre/mood matches, so closely related tastes (e.g. `rock` vs `hard rock`) score no partial credit, and it ignores tempo, valence, danceability, and acousticness entirely.

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Sample Recommendation Output

Paste a sample of your recommender's output here as a text block so a reader can see what it produces:

User profile: `{"genre": "pop", "mood": "happy", "energy": 0.8}`

```
Loaded songs: 20

Top recommendations:

Sunrise City - Score: 3.98
Because: genre match: pop (+2.0); mood match: happy (+1.0); energy close to 0.8 (+0.98)

Gym Hero - Score: 2.87
Because: genre match: pop (+2.0); energy close to 0.8 (+0.87)

Rooftop Lights - Score: 1.96
Because: mood match: happy (+1.0); energy close to 0.8 (+0.96)

Go Johnny Go - Score: 1.95
Because: mood match: happy (+1.0); energy close to 0.8 (+0.95)

Love Train - Score: 1.90
Because: mood match: happy (+1.0); energy close to 0.8 (+0.90)
```

**Screenshot or video** *(optional)*: <!-- Insert a screenshot or demo video link here -->

---

## Experiments You Tried

**Weight shift (genre 2.0 → 1.0, energy 1.0 → 2.0), Deep Intense Rock profile:**

The top-5 order stayed almost identical, but the scores compressed. The gap between genre-matched songs and mood-only songs nearly closed — *Rainbow in the Dark* (mood + energy, no genre match) rose from 1.95 to 2.90, just behind *Ball Breaker* (2.94). This showed the genre weight was doing most of the ranking work: making energy the dominant term produced results that were *different* rather than clearly *more accurate*, since for a rock fan the genre-dominant baseline still felt more correct.

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this



