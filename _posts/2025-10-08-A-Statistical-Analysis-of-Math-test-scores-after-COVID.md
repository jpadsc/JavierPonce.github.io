## Did COVID make students attempting to go to college worse at math?

As a student who experienced the online classes era during COVID, I believe that online schooling significantly decreased learning among all students, especially in abstract subjects like mathematics. Similarly, during my years as a Mathematics and Data Science tutor at UCSD, I found that many students struggled due to a lack of understanding of fundamental concepts they had studied during the COVID pandemic. Driven by this belief, I started looking for relevant data to run statistical tests on.

Ideally, when you want to test a causal effect like the one I described in the above paragraph, you would seek data that allows you to hold all variables constant while changing the variable that causes the effect. Finding such data would require students who were and weren’t affected by the COVID pandemic at the same time, which is impossible without access to a parallel reality where COVID didn’t happen. Instead, we can compare how students performed during math tests before and after the pandemic. While such a comparison won't prove causality, if we don’t find any statistically significant change in performance, then it doesn’t make sense to hold a belief that is not somehow reflected in our reality. Thus, I decided to run statistical tests on data from the SAT yearly reports from 2020 to 2025. 

## The parameters of interest and the Data.

Before running any tests, let me define the parameters of interest and explain how we can use the data from the yearly SAT reports to test the defined parameters. I’ll define $\mu_x$ to be the average math SAT score obtained by high school students who want to attend college in the near future during the school cycle $x$ in the hypothetical case where all of them took the test. For simplicity, let each school cycle be represented by the latter year of each cycle, meaning that $\mu_{2025}$ represents the true average score of all students who want to attend college in the 2025-2024 cycle if we were to test all of these students. Under these definitions, we are interested in testing the hypothesis that  $\mu_x < \mu_y$, where $x$ represents a school cycle after the COVID lockdown and $y$ the one before it. 

Since not all students attempting to attend college in each school cycle take the SAT, the data from the SAT yearly reports are from a sample of our population of interest. Now, using the data from our sample, we can run statistical tests to make inferences about our parameters of interest. There’s just one problem. If you are familiar with statistics, you are probably concerned that our sample of students who took the SAT isn’t random, but instead the majority self-select to take or not take the test. Then how can I run meaningful tests on this data?

### A not-so-random sample.

A sample of self-selected individuals is likely to be biased because the participants might use specific information about themselves to decide whether or not to participate in the experiment. In Economics, adverse selection is a famous example of how self-selection can introduce biases to a group. Essentially, when studying the models of an insurance company that sells non-mandatory insurance, we conclude that individuals who purchased insurance have a higher probability of being granted the benefits of their insurance than the population. This happens because each individual has knowledge about their probability of being granted the benefits of the insurance, and they use this information before making the decision to purchase insurance. The reason I bring up this specific example is that even though self-selection caused a bias, Economic theory allows us to predict the overall effect of self-selection in that sample. Therefore, I’ll use ideas from Economics to predict the overall effect of self-selection in our sample of students who took the SAT.

Let’s think of the utility function that a student would consider before making the decision to take the SAT. Taking the SAT represents a cost; it requires money, preparation, and I think it is safe to assume that most students consider taking a test costly. On the other hand, students also gain utility after taking the SAT. In particular, we can split the gained utility into two groups: utility that depends on their score and utility that is independent of it. Then, to decide if they should take the SAT, students consider the following utility function.

$$ U(score) = B_1 + B_2(score) - C $$

Where $B_1$ is the part of your gained utility independent of your score, and $B_2$ is the dependent part. $C$ is just the utitlity lost from paying the costs.


$B_1$ and $C$ should be known to the student even before they decide to take the SAT, so these are constants unique to each student. Now, the problem with this utility function is that students have a somewhat accurate estimate of what their score would be, and thus, they have an estimate for $B_2$ and $U$. Therefore, students who estimate high scores for themselves are more likely to take the SAT than students who estimate lower scores for themselves. This suggests that the average scores that we see in the reports should be higher than the average score we would see if every student from the relevant generation took the SAT.  Because of this bias, I shifted my population of interest from all high school students in the relevant generation to high school students who want to attend college in the near future (from the same relevant generation). My argument is that this subset of students has such a strong desire to attend college that the constant $B_1$ undermines the additional utility they’ll get from $B_2$. Students with these high $B_2$’s no longer depend on their score estimates to decide if they’ll take the SAT; instead, they’ll only decide not to take it upon random instances of an equally high $C$, like getting really sick the day you scheduled your exam. Under this model, the sample is random.

*Small note: Why does it make sense for  $B_1$ to be large  in our population of interest ? Applications that require/request SAT scores are a reason for $B_1$ to be high, but determined students have a lot to gain from taking the SAT even if they believe they’ll do poorly.*

This argument is especially strong for the pre-COVID pandemic era, when most universities required SAT scores for the application process. Unfortunately, many universities stopped considering SAT scores during the COVID-19 pandemic, which lowers the $B_1$ value. While schools have shifted back to requiring/requesting test scores in recent years, I believe it is a reasonable assumption that $ B_1$’s are still lower than before COVID. This leaves us with two scenarios: either the post-COVID $B_1$’s are high enough to believe that the sample is random under my original argument, or the post-COVID $B_1$’s are not high enough for my argument, and we can’t assume randomness. In the latter case, the pre-COVID sample can be thought of as random, but  the post-COVID sample is biased towards having a bigger mean than the population. Even if we were to run a test on this biased data, because our hypothesis was that $\mu_x < \mu_y$, where $x$ represents a school cycle after the COVID lockdown and $y$ the one before it. Then, rejecting the null of $\mu_x = \mu_y$ with such bias would mean that our test is even more significant after removing the bias. Intuitively, because the specified bias makes it harder for us to reject the null, if we somehow manage to reject it anyway, the test becomes even more significant. 

Hopefully, my arguments help you understand why I think it is reasonable to take all the data as if it were randomly sampled. I understand that I am making a lot of assumptions here, and I would like to hear your thoughts about this section in the comments (assuming I implemented comments by the time I publish this page). That said, for the remainder of this analysis, I will treat the samples as if they were random in order to run statistical tests.

### Is that normal?

In order to run tests, you also need to have information about the type of distribution your data is coming from. Ideally, if you have access to all the data, you can use the data to infer the distribution of the data. For example, if I wanted to show the distribution is approximately normal, I would create a Q-Q plot or use the Kolmogorov-Smirnov test. Unfortunately, I don’t have access to the complete sample, but only the reported summary. Thankfully, it is a popular assumption to consider SAT scores to be approximately normal, so for the rest of this analysis, I’ll assume that the samples come from a normal distribution with unknown parameters $\mu$ and $\sigma$.  

### Table summary of the data.

| School Cycle | Mean | Standard deviation | # of participants |
|--------------|------|--------------------|-------------------|
|2025-2024     |508   | 126                | 2,004,965         |
|2024-2023     |505   | 125                | 1,973,891         |
|2023-2022     |508   | 122                | 1,913,742         |
|2020-2019     |523   | 117                | 2,198,460         |


## Time to test the test.


