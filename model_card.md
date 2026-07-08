# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

Give your model a short, descriptive name.  
Example: **VibeFinder 1.0**  

---

## 2. Intended Use  

Describe what your recommender is designed to do and who it is for. 

Prompts:  

- What kind of recommendations does it generate  
- What assumptions does it make about the user  
- Is this for real users or classroom exploration  

---

## 3. How the Model Works  

Explain your scoring approach in simple language.  

Prompts:  

- What features of each song are used (genre, energy, mood, etc.)  
- What user preferences are considered  
- How does the model turn those into a score  
- What changes did you make from the starter logic  

Avoid code here. Pretend you are explaining the idea to a friend who does not program.

---

## 4. Data  

Describe the dataset the model uses.  

Prompts:  

- How many songs are in the catalog  
- What genres or moods are represented  
- Did you add or remove data  
- Are there parts of musical taste missing in the dataset  

---

## 5. Strengths  

Where does your system seem to work well  

Prompts:  

- User types for which it gives reasonable results  
- Any patterns you think your scoring captures correctly  
- Cases where the recommendations matched your intuition  

---

## 6. Limitations and Bias 

Where the system struggles or behaves unfairly. 

Prompts:  

- Features it does not consider  
- Genres or moods that are underrepresented  
- Cases where the system overfits to one preference  
- Ways the scoring might unintentionally favor some users  

**What I found:** The scoring always awards energy points to *every* song, so a track with the wrong genre and wrong mood can still climb the list on energy alone. In the Chill Lofi test, *Catch the Rainbow* — a rock ballad — landed in the top 5 purely because its energy (0.35) exactly matched the target, despite having nothing to do with lofi. The catalog is also rock-heavy (5 of 20 songs are rock, hard rock, or metal) while genres like pop and jazz have only one or two entries, so genre-weighted profiles for the larger genres fill their whole top list with matches while smaller-genre fans run out of real matches quickly. Because the system only rewards *exact* genre and mood matches, closely related tastes (for example `rock` vs `hard rock`) get no partial credit, which unfairly penalizes users whose taste sits between the labels. Together these create a mild filter bubble that favors well-represented genres and lets raw energy leak across genre boundaries.

---

## 7. Evaluation  

How you checked whether the recommender behaved as expected. 

Prompts:  

- Which user profiles you tested  
- What you looked for in the recommendations  
- What surprised you  
- Any simple tests or comparisons you ran  

No need for numeric metrics unless you created some.

**Profiles tested:** three distinct tastes — High-Energy Pop (pop / happy / 0.8), Deep Intense Rock (hard rock / intense / 0.85), and Chill Lofi (lofi / chill / 0.35). For each, I looked at whether the top 5 matched what a real fan of that style would actually want.

**What surprised me:** the biggest surprise was a rock ballad, *Catch the Rainbow*, showing up in the Chill Lofi list. It got there on energy alone, which taught me that a feature everyone always scores on (energy) can quietly pull in songs that don't fit the vibe at all.

**Profile comparisons (plain language):**

- **High-Energy Pop vs. Deep Intense Rock:** The pop profile floats bright, upbeat songs (Sunrise City, Gym Hero) to the top, while the rock profile pulls up loud, hard-hitting tracks (Civil War, Dirty Deeds, Ball Breaker). This makes sense — the two profiles ask for opposite moods (happy vs. intense) and different genres, so almost none of their top songs overlap.
- **Deep Intense Rock vs. Chill Lofi:** These are near opposites in energy (0.85 vs. 0.35). The rock list is full of fast, high-energy songs; the lofi list is full of slow, calm ones (Library Rain, Midnight Coding). The energy gap alone flips the whole ranking, which is exactly what you'd expect when one person wants a workout and the other wants to study.
- **High-Energy Pop vs. Chill Lofi:** Both can feel "pleasant," but pop wants high energy and happy mood while lofi wants low energy and chill mood. The pop list leans danceable and loud; the lofi list leans quiet and mellow. The only thing they share is that energy nudges a few unrelated songs onto both lists.

To put it simply: the reason "Gym Hero" keeps showing up for the Happy Pop fan is that it matches their exact genre *and* sits near their energy target — the system is rewarding the two things that person actually asked for, not judging the song's quality.

### Stress Test Output

Three diverse profiles were run against the 20-song catalog (top 5 each):

```
=== High-Energy Pop: {'genre': 'pop', 'mood': 'happy', 'energy': 0.8} ===
Sunrise City                 - 3.98  genre match: pop (+2.0); mood match: happy (+1.0); energy close to 0.8 (+0.98)
Gym Hero                     - 2.87  genre match: pop (+2.0); energy close to 0.8 (+0.87)
Rooftop Lights               - 1.96  mood match: happy (+1.0); energy close to 0.8 (+0.96)
Go Johnny Go                 - 1.95  mood match: happy (+1.0); energy close to 0.8 (+0.95)
Love Train                   - 1.90  mood match: happy (+1.0); energy close to 0.8 (+0.90)

=== Deep Intense Rock: {'genre': 'hard rock', 'mood': 'intense', 'energy': 0.85} ===
Civil War                    - 3.90  genre match: hard rock (+2.0); mood match: intense (+1.0); energy close to 0.85 (+0.90)
Dirty Deeds Done Dirt Cheap  - 3.00  genre match: hard rock (+2.0); energy close to 0.85 (+1.00)
Ball Breaker                 - 2.97  genre match: hard rock (+2.0); energy close to 0.85 (+0.97)
Rainbow in the Dark          - 1.95  mood match: intense (+1.0); energy close to 0.85 (+0.95)
Storm Runner                 - 1.94  mood match: intense (+1.0); energy close to 0.85 (+0.94)

=== Chill Lofi: {'genre': 'lofi', 'mood': 'chill', 'energy': 0.35} ===
Library Rain                 - 4.00  genre match: lofi (+2.0); mood match: chill (+1.0); energy close to 0.35 (+1.00)
Midnight Coding              - 3.93  genre match: lofi (+2.0); mood match: chill (+1.0); energy close to 0.35 (+0.93)
Focus Flow                   - 2.95  genre match: lofi (+2.0); energy close to 0.35 (+0.95)
Spacewalk Thoughts           - 1.93  mood match: chill (+1.0); energy close to 0.35 (+0.93)
Catch the Rainbow            - 1.00  energy close to 0.35 (+1.00)
```

---

## 8. Future Work  

Ideas for how you would improve the model next.  

Prompts:  

- Additional features or preferences  
- Better ways to explain recommendations  
- Improving diversity among the top results  
- Handling more complex user tastes  

---

## 9. Personal Reflection  

A few sentences about your experience.  

Prompts:  

- What you learned about recommender systems  
- Something unexpected or interesting you discovered  
- How this changed the way you think about music recommendation apps  
