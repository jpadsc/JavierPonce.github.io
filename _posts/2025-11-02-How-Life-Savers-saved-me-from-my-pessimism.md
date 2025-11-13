---
header:
    image: /assets/images/lifesaversimage.png
    image_description: "life savers gummies"
---
## No more watermelon sugar.

I don't like watermelon. Naturally, I also dislike watermelon-flavored candy, but for some reason, whenever I enjoy a bag of Life Savers gummies, I feel as if the majority of the gummies I pull out of the bag are watermelon-flavored. This feeling made me wonder if the five flavors included in the bag appeared in equal proportions, and if watermelon-flavored gummies were as abundant as I perceived them to be. Thus, after finishing the bag that sparked all these questions in me, I ran to the store to buy more Life Savers to perform statistical tests on the samples from the new bags.

## The parameters of interest and the Data.

Contrary to what the AI-generated header image depicts, the Life Savers gummies I consume in the US come in five flavors: Cherry (dark red), Strawberry (light red), Green Apple (dark green), Watermelon (light green), and Orange. Then we can define $p_{C}$, $p_{S}$, $p_{A}$, $p_{W}$, and $p_{O}$ to be the proportions of produced gummies that belong to each flavor group, respectively. Notice that under these definitions, our samples come from a Multinomial distribution with parameters $(n, p_{C}, p_{S}, p_{A}, p_{W}, p_{O})$.

### The sweet data.

![separated gummies](../assets/images/gummies.jpeg)

After separating and counting all the gummies of each flavor by hand, I organized the data in the following table.

|Bag   |Apple |Orange |Strawberry |Cherry |Watermelon |
|------|------|-------|-----------|-------|-----------|
|Bag 1 | 9    | 10    | 7         | 20    | 4         |
|Bag 2 | 14   | 6     | 9         | 16    | 5         |
|Both  | 23   | 16    | 16        | 36    | 9         |

Now let's use the data to make inferences about the population parameters.
