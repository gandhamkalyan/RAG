>Annoy stands for Approximate Nearest Neighbors Oh Yeah. It is a library developed by Spotify for finding vectors that are similar to a query vector quickly.

> The key word is Approximate.

> Instead of comparing your query with every single vector, Annoy builds an index that lets it search only the most promising parts of the vector space.

> An index is a data structure that helps you find information faster without checking every item one by one.

> To understand Annoy, imagine trying to find the most similar food item to a Cheeseburger in a database of 1,000 foods based on two features: Calories and Price.1. Building the Tree (Splitting the Space)Annoy draws random lines to divide your data points step-by-step.   

                  \[ All 1,000 Food Items ]

                            │

                 Is Price > ₹150? (Split 1)

                    /               \\

                 \[Yes]             \[No]

                  /                 \\

    Is Calories > 500? (Split 2)   ...and so on

            /          \\

     \[Leaf Node A]   \[Leaf Node B]
     (Pizza, Burger) (Salad, Sushi)



Step 1: Annoy picks a random line to split the dataset. Let’s say it separates expensive items from cheap items.
Step 2: It looks at the expensive items and draws another random line, separating high-calorie items from low-calorie items.
Step 3: It repeats this until only a few items are left in a group. These final groups are called Leaf Nodes.2. 

The Problem with One Tree

Because the lines are drawn randomly, a Cheeseburger and a Chicken Sandwich might end up on opposite sides of a line just by pure luck, even though they are very similar.

To fix this, Annoy builds multiple trees (a forest). Each tree uses different random lines to slice up the exact same data.

. Querying (Finding the Match)

Now, you search for the closest match to a Cheeseburger:

Drop it down Tree 1: It lands in Leaf Node A (contains: Pizza, Cheeseburger).
Drop it down Tree 2: It lands in a different Leaf Node (contains: Chicken Sandwich, Cheeseburger).
Collect Candidates: Annoy pools the results from all trees together: \[Pizza, Chicken Sandwich].

Exact Distance: Annoy calculates the exact mathematical distance only for those two candidates, instead of checking all 1,000 items in the database.It instantly identifies the Chicken Sandwich as the closest match.

