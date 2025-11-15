---
header:
    image: /assets/images/lifesaversimage.png
    image_description: "life savers gummies"
---

# How Life Savers saved me from my pessimism.

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

## Not all the flavors are created equal.

After deciding to run these statistical tests, my first hypothesis was that all gummy flavors appeared in equal proportions and that my brain exaggerated the amount of watermelon Life Savers I encountered. Formally, my hypothesis was that $p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$. Since our data comes from a multinomial distribution, we can use an approximation to perform a chi-square goodness-of-fit test to test such a hypothesis. Still, as in most cases where you plan to use an approximation, you need to check that such an approximation is relevant to your specific case. In this case, we need $np_{i0} \geq 5$ for all i, where $p_{i0}$ is the proportion of  "i" flavored Life Savers under the null hypothesis. The null hypothesis in this case is $H_0 : p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$, thus $ n \geq 25$. Since we have a sample of 100 gummies, it is sound for us to use the approximation. 

### Testing $p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$.

$H_0 : p_{C} = p_{S} = p_{A} = p_{W} = p_{O} = \frac{1}{5}$ ; $ H_1: p_i \neq p_j$ for some i and j.

Under the null
$d = \sum_{i \in flavors} \frac{(k_i - np_{i0})^2}{np_{i0}} \sim \chi_4^2$.

Then, we reject the null if our p-value ($P(\chi_4^2 \geq d)$) is lower than the chosen significance level $\alpha$, which in this case I decided to use a standard $\alpha = 0.05$.

Using the SciPy Python library, we get that $d = 20.9$, and that the corresponding p-value is $0.00033145 < \alpha$. Therefore, we successfully reject the null hypothesis, meaning that the data suggest that not all Life Saver flavors are produced in equal amounts.

This test result came as a huge surprise. Firstly, because I never imagined that Mars (the company that produces Life Savers) would find a reason to focus production on some flavor more than others, but even more surprising, because assuming that the null hypothesis is wrong implies some flavor (or group of flavors) represents significantly less than $\frac{1}{5}$ of all produced gummies, and looking at the data I now believe that's the case for Watermelon Life Savers, contrdicting my original motivating tought. Driven by this implication, I'll now test if the proportion of Watermelon Life Savers is significantly lower than $\frac{1}{5}$, using a significance level of $\alpha = 0.05$, and I will also find a confidence interval for the value of this proportion.

## Exploring the Watermelon distribution.

While the data on how many gummies come from each flavor comes from a multinomial distribution, you may recall that each component of a multinomial is a binomial distribution. Thus, the number of Watermelon Life Savers in our sample comes from a Binomial$(100, p_W)$, and we can use this to do inference about $p_W$.

When testing a hypothesis about the parameter $p$ of a binomial random variable, I would use a normal approximation of the binomial. Using such an approximation is extremely helpful because it drastically reduces the necessary computational work, without destroying the test's accuracy. Thankfully, SciPy comes with the function binomtest(), which can perform this hypothesis test using the exact probability mass function of a binomial random variable. Since our sample is relatively small, running binomtest() shouldn't be a problem for my computer, and it is a way to perform the hypothesis test without compromising any of the test's accuracy. Now, let's run the hypothesis test using binomtest().

### Testing $p_W < \frac{1}{5}$.

$H_0: p_W = \frac{1}{5} $ ; $H_1: p_W < \frac{1}{5}$

Under the null
$K_w \sim Binom(100, \frac{1}{5})$ , where $K_w$ is the number of Watermelon Life Savers from the sample.

Thus we reject the null at the significance level $\alpha = 0.05$ if 
$P(Binom(100, \frac{1}{5}) \leq 9) \leq \alpha$.

Using python we get that
$P(Binom(100, \frac{1}{5}) \leq 9) = 0.0023335$.
Therefore, we reject the null, meaning that the data suggest that watermelon-flavored Life Savers appear at a significantly lower rate than $\frac{1}{5}$.

### 95% Confidence Interval for $p_W$.

Now that we have enough evidence to believe that $p_W < \frac{1}{5} $, I am interested in estimating $p_W$. While the maximum likelihood estimate is $0.09$ a more useful piece of information would be a 95% confidence interval, which we can easily get with binomtest().proportion_ci().

After running the code, we are 95% confident that $p_W \in (0, 0.15179)$. Again, these results are the complete opposite of what I perceived while consuming these gummies, leaving me astonished.

## Tampered bags.

So far, I've been assuming that each bag represents a random sample of the population of Life Saver gummies, which is why I've been able to consider the two bags as just one bigger sample. Making such an assumption is natural, but as a last resort to validate my original perception of $p_W$, I'll consider the possibility that my assumption wasn't sound. Perhaps during the packaging process, there's an individual in charge of deciding how to build a bag of gummies, and this individual deliberately chooses to make bags with significantly different flavor distribution. Therefore, in this last section, I'll test if the two bags I bought come from the same distribution. In other words, I will test independence between the events $F_i$ and $B_j$, where $F_i$ is the event of a gummy being of flavor $i$, and $B_j$ represents a gummy coming from bag $j$. 

### Testing indepence for $F_i$ and $B_j$.

This test is essentially a goodness of fit test with estimated parameters.

$H_0: F_i$ and  $B_j$ are independent ; $H1: F_i$ and $B_j$ are dependent.

Under the null
$ d_2 = \sum_{i \in flavors} \sum_{j=1}^2 \frac{ ( k_{ij} - 50\hat{p}_i)^2 }{50\hat{p}_i} \sim \chi_4^2 $

where $k_{ij}$ is the number of elements in $F_i \cap B_j$, and  $\hat{p}_i$ is the maximum likelihood estimate of $P(F_i)$.

Thus, we reject the null at the significance level $\alpha = 0.05$ if $P(\chi_4^2 \geq d_2) \leq \alpha$.

After computing the test in python, we get that $d_2 = 2.8925$ and $P(\chi_4^2 \geq d_2) = 0.57597 > \alpha$. Therefore, we fail to reject the null, meaning that the data from each bag of gummies is similar enough for us to believe that it comes from the same distribution. While this test only addresses the relation between the two bags I bought, it is reasonable to assume that other bags will follow the same distribution since they all pass through the same packaging process.

## Final thoughts.

Through this statistical analysis, I conclude that not all flavors of Life Savers gummies appear in equal amounts. After realizing that Mars produces smaller quantities of the Watermelon-flavored gummies, which I dislike, I now theorize that perhaps the popular flavors appear more often than the less popular ones. To test this theory, I would need a random sample of Life Savers consumers to rank these five flavors, which I don't have access to. Nevertheless, I find the implication that Mars has a reason to behave this way very interesting, and I wonder about the economic models they considered before making that decision. 

Lastly, on a more personal note, my tests made me realize how pessimistic I can be at times. I perceived watermelon-flavored gummies to be the majority of the bunch just because of how much I dislike them, and I couldn't have been more wrong about that. Realizing how easy it is to exaggerate a bad experience encouraged me to be more positive and grateful for everything in my life. Perhaps it is because Thanksgiving is near, but I'd like to invite the reader to keep a positive mindset and look for things they are grateful for.

Thank you for reading. If you are interested in reading the python notebook I created for all the computations in this post [click here](https://github.com/jpadsc/Chi_sq_Goodness_of_fit_test_on_life_savers/blob/main/tests.ipynb).