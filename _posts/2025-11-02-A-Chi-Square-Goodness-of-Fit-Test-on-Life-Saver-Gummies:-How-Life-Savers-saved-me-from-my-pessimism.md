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

## Not all the flavors are made equal.

After deciding to run these statistical tests, my first hypothesis was that all gummy flavors appeared in equal proportions and that my brain exaggerated the amount of watermelon Life Savers I encountered. Formally, my hypothesis was that $p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$. Since our data comes from a multinomial distribution, we can use an approximation to perform a chi-square goodness-of-fit test to test such a hypothesis. Still, as in most cases where you plan to use an approximation, you need to check that such an approximation is relevant to your specific case. In this case, we need $np_{i0} \geq 5$ for all i, where $p_{i0}$ is the proportion of  "i" flavored Life Savers under the null hypothesis. The null hypothesis in this case is $H_0 : p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$, thus $ n \geq 25$. Since we have a sample of 100 gummies, it is sound for us to use the approximation. 

### Testing $p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$

$H_0 : p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$ ; $ H_1: p_i \neq p_j$ for some i and j.

Under the null
$d = \sum_{i \in flavors} \frac{(k_i - np_{i0})^2}{np_{i0}} \sim \chi_4^2$