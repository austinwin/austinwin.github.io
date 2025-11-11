---
layout: post
title: What the Transformer Does?
---
A Transformer is like a smart classroom.  
Every word in a sentence is a student sitting in class.  
Each student can look around at others and decide who to listen to before writing down their own understanding.  
That “looking around and deciding who to listen to” part is called: Seft-attention  
Embedding: Turning Words into Numbers  
Before words can talk to each other, they must become numbers.  
The computer can’t understand “cat” or “dog”, but it can understand something like:  
| Word | Embedding (tiny example) |
| ---- | ------------------------ |
| cat  | [0.2, 0.8]               |
| dog  | [0.3, 0.7]               |
| car  | [0.9, 0.1]               |
These are just coordinates — like placing “cat” and “dog” close to each other on a map because they mean similar things.

🧠 Why it matters:
Embeddings give every word a shape of meaning.

🎲 Kid analogy:
Every toy has a color and size. “Cat” and “dog” are both small and fluffy; “car” is big and shiny. The colors and sizes are like embedding numbers.
