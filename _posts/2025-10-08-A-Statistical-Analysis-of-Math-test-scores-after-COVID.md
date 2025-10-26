---
toc: true
toc_label: "Table of Contents"
toc_sticky: true
---

## Did COVID make students attempting to go to college worse at math?

As a student who experienced the online classes era during COVID, I believe that online schooling significantly decreased learning among all students, especially in abstract subjects like mathematics. Similarly, during my years as a Mathematics and Data Science tutor at UCSD, I found that many students struggled due to a lack of understanding of fundamental concepts they had studied during the COVID pandemic. Driven by this belief, I started looking for relevant data to run statistical tests on.

Ideally, when you want to test a causal effect like the one I described in the above paragraph, you would seek data that allows you to hold all variables constant while changing the variable that causes the effect. Finding such data would require students who were and weren’t affected by the COVID pandemic at the same time, which is impossible without access to a parallel reality where COVID didn’t happen. Instead, we can compare how students performed during math tests before and after the pandemic. While such a comparison won't prove causality, if we don’t find any statistically significant change in performance, then it doesn’t make sense to hold a belief that is not somehow reflected in our reality. Thus, I decided to run statistical tests on data from the SAT yearly reports from 2020 to 2025. 

## The parameters of interest and the Data.

Before running any tests, let me define the parameters of interest and explain how we can use the data from the yearly SAT reports to test the defined parameters. I’ll define $\mu_x$ to be the average math SAT score obtained by high school students who want to attend college in the near future during the school cycle $x$ in the hypothetical case where all of them took the test. Similarly, define $\sigma_x$ to be the standard deviation of the math SAT scores obtained by high school students who want to attend college in the near future during the school cycle $x$ in the hypothetical case where all of them took the test. For simplicity, let each school cycle be represented by the most recent year of each cycle, meaning that $\mu_{2025}$ represents the true average score of all students who want to attend college in the 2025-2024 cycle if we were to test all of these students. Under these definitions, we are interested in testing the hypothesis that  $\mu_x < \mu_y$, where $x$ represents a school cycle after the COVID lockdown and $y$ the one before it. 

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

As I am interested in testing the hypothesis $H_1 : \mu_x < \mu_y$ against the null $H_0 : \mu_x = \mu_y$, I will do a two-population t-test using the data from the table in the previous section. Before starting the test, I need to decide whether it is reasonable to assume equal variance or if I’ll use Welch’s approximation to continue forward. Thus, I’ll start with an equal variance F-test for every cycle after the COVID pandemic compared to the cycle immediately before it.

### F-tests for equal variance

Normally, I would run these tests using built-in functions from Python, but most of these require access to all data, which I don’t have. Instead, for all the tests in this analysis, I’ll use fucntions I wrote in R to use whenever I deal with summarized data.

This is my personalized function for the following F-tests. 

```
equal_var_test <- function(S_x, n, S_y, m, alpha){
  f <- (S_y^2)/(S_x^2)
  F_l <- qf( (alpha/2), (m-1), (n-1) )
  F_u <- qf( (alpha/2), (m-1), (n-1), lower.tail = FALSE)
  cat("f is :", f, "F_l:", F_l, "F_u", F_u, "\n")
  if( f <= F_l ) {
      return("reject the null")
  } else if(f >= F_u){
    return("reject the null")
  } else{
    return("fail to reject the null")
  }
  
}
```

#### Testing $\sigma_{2025} \neq \sigma_{2020}$

$$H_0 : \sigma_{2025} = \sigma_{2020} \quad ; \quad H_1: \sigma_{2025} \neq \sigma_{2020}$$

Under the null hypothesis $H_0$

$$ f = \frac{S_{2020}^2}{S_{2025}^2} \sim F_{2198459,2004964}$$.

Where $S_x$ represents the unbiased estimator of the standart deviation for cycle $x$.

Then, after computing f with the values from the data, we reject the null at the significance level $\alpha = 0.05$ if either (1) $f \leq F_{\frac{\alpha}{2},2198459,2004964}$ or (2) $ f \geq F_{1 - \frac{\alpha}{2},2198459,2004964}$.

Using my function and the values from the report, we get that (1)$f = 0.8622449 < F_{0.025,2198459,2004964} = 0.9980453$.

Therefore, the test rejects the null hypothesis meaning I should use Welch's approximation for the two population mean test.

Function's output:
```
> equal_var_test(S_2025, n_2025, S_2020, n_2020, 0.05)
f is : 0.8622449 F_l: 0.9980453 F_u 1.00196 
[1] "reject the null"
```

#### Testing $\sigma_{2024} \neq \sigma_{2020}$

$$H_0 : \sigma_{2024} = \sigma_{2020} \quad ; \quad H_1: \sigma_{2024} \neq \sigma_{2020}$$

Under the null hypothesis $H_0$

$$ f = \frac{S_{2020}^2}{S_{2024}^2} \sim F_{2198459,1973890}$$.

Therefore, we reject the null at the significance level $\alpha = 0.05$ if either (1) $f \leq F_{\frac{\alpha}{2},2198459,1973890}$ or (2) $ f \geq F_{1 - \frac{\alpha}{2},2198459,1973890}$.

Using my function and the values from the report, we get that (1)$f = 0.876096 < F_{0.025,2198459,1973890} = 0.99803$.

Therefore, the test rejects the null hypothesis meaning I should use Welch's approximation for the two population mean test.

Function's output:
```
> equal_var_test(S_2024, n_2024, S_2020, n_2020, 0.05)
f is : 0.876096 F_l: 0.99803 F_u 1.001976 
[1] "reject the null"
```

#### Testing $\sigma_{2023} \neq \sigma_{2020}$

$$H_0 : \sigma_{2023} = \sigma_{2020} \quad ; \quad H_1: \sigma_{2023} \neq \sigma_{2020}$$

Under the null hypothesis $H_0$

$$ f = \frac{S_{2020}^2}{S_{2023}^2} \sim F_{2198459,1913741}$$.

Therefore, we reject the null at the significance level $\alpha = 0.05$ if either (1) $f \leq F_{\frac{\alpha}{2},2198459,1913741}$ or (2) $ f \geq F_{1 - \frac{\alpha}{2},2198459,1913741}$.

Using my function and the values from the report, we get that (1)$f = 0.9197124 < F_{0.025,2198459,1913741} = 0.9979994 $.

Therefore, the test rejects the null hypothesis meaning I should use Welch's approximation for the two population mean test.

Function's output:
```
> equal_var_test(S_2023, n_2023, S_2020, n_2020, 0.05)
f is : 0.9197124 F_l: 0.9979994 F_u 1.002007 
[1] "reject the null"
```

#### Conclusion of the tests.

According to the F tests, none of the pairs share variance. Thus, I will use Welch’s approximation to run the two-population mean t-tests.

### Two-Sample mean t-tests.

Finally, we get to test my original hypothesis $H_1 : \mu_x < \mu_y$ against the null $H_0 : \mu_x = \mu_y$. As concluded in the previous section, I won’t assume equal variances, so I’ll have to approximate the degrees of freedom using Welch’s approximation. Once again, I’ll be using an R function I wrote to run the two-sample t-test.

My function:
```
lower_mean_t_test <- function(X_bar, n, S_x, Y_bar, m, S_y, alpha){
  t <- (X_bar - Y_bar)/( ( ((S_x^2)/n)+((S_y^2)/m) )^(1/2) )
  theta <- (S_x^2)/(S_y^2)
  v <- ((theta + (n/m))^2)/( ((theta^2)/(n-1)) + (((n/m)^2)/(m-1)) )
  p_value <- pt(t, round(v))
  cat("t_stat:", t, ", v:", round(v),", cutoff", qt(alpha, round(v)), ", pvalue:", p_value, "\n")
  if(p_value <= alpha){
    return("reject the null, mean x is significantlly smaller than mean y")
  } else{
    return("fail to reject the null")
  }
}
```

#### Testing $\mu_{2025} < \mu_{2020}$.

$$ H_0 : \mu_{2025} = \mu_{2020} \quad ; \quad H_1 : \mu_{2025} < \mu_{2020} $$.

Under the null $H_0$, and using Welch's approximation 

$$ t = \frac{ \bar{X}_{2025} - \bar{X}_{2020} }{\sqrt{ \frac{S_{2025}^2}{2004965} + \frac{S_{2020}^2}{2198460} }} \sim T_v $$.

Where using $ \theta = \frac{S_{2025}^2}{S_{2020}^2} $, $ v = \frac{(\theta + \frac{2004965}{2198460})^2}{\frac{\theta^2}{2004964} + \frac{2004965^2}{2198460^2 (2198459)}} $.

Therefore, we reject the null at the significance level $\alpha = 0.05$ if $t \leq T_{\alpha,v}$.

Using my function and the values from the report, we get that

$$ t = -126.1218, v = 4090955 ; t < T_{0.05,v} = -1.644854 $$ 

Thus, the tests rejects the null, meaning that the sample from the school cycle 2025-2024 has significantly lower math scores on average than the one from 2020-2019 (pre-COVID).

Function's output:
```
lower_mean_t_test(avg_2025,n_2025,S_2025, avg_2020,n_2020,S_2020, 0.05)
t_stat: -126.1218 , v: 4090955 , cutoff -1.644854 , pvalue: 0 
[1] "reject the null, mean x is significantlly smaller than mean y"
```
#### Testing $\mu_{2024} < \mu_{2020}$.

$$ H_0 : \mu_{2024} = \mu_{2020} \quad ; \quad H_1 : \mu_{2024} < \mu_{2020} $$.

Under the null $H_0$, and using Welch's approximation 

$$ t = \frac{ \bar{X}_{2024} - \bar{X}_{2020} }{\sqrt{ \frac{S_{2024}^2}{1973891} + \frac{S_{2020}^2}{2198460} }} \sim T_v $$.

Where using $ \theta = \frac{S_{2024}^2}{S_{2020}^2} $, $ v = \frac{(\theta + \frac{1973891}{2198460})^2}{\frac{\theta^2}{1973890} + \frac{1973891^2}{2198460^2 (2198459)}} $.

Therefore, we reject the null at the significance level $\alpha = 0.05$ if $t \leq T_{\alpha,v}$.

Using my function and the values from the report, we get that

$$ t = -151.3596, v = 4050399 ; t < T_{0.05,v} = -1.644854 $$ 

Thus, the tests rejects the null, meaning that the sample from the school cycle 2024-2023 has significantly lower math scores on average than the one from 2020-2019 (pre-COVID).

Function's output:
```
> lower_mean_t_test(avg_2024,n_2024,S_2024, avg_2020,n_2020,S_2020, 0.05)
t_stat: -151.3596 , v: 4050399 , cutoff -1.644854 , pvalue: 0 
[1] "reject the null, mean x is significantlly smaller than mean y"
```

#### Testing $\mu_{2023} < \mu_{2020}$.

$$ H_0 : \mu_{2023} = \mu_{2020} \quad ; \quad H_1 : \mu_{2023} < \mu_{2020} $$.

Under the null $H_0$, and using Welch's approximation 

$$ t = \frac{ \bar{X}_{2023} - \bar{X}_{2020} }{\sqrt{ \frac{S_{2023}^2}{1913742} + \frac{S_{2020}^2}{2198460} }} \sim T_v $$.

Where using $ \theta = \frac{S_{2023}^2}{S_{2020}^2} $, $ v = \frac{(\theta + \frac{1913742}{2198460})^2}{\frac{\theta^2}{1913741} + \frac{1913742^2}{2198460^2 (2198459)}} $.

Therefore, we reject the null at the significance level $\alpha = 0.05$ if $t \leq T_{\alpha,v}$.

Using my function and the values from the report, we get that

$$ t = -126.7547, v = 3982576 ; t < T_{0.05,v} = -1.644854 $$ 

Thus, the tests rejects the null, meaning that the sample from the school cycle 2023-2022 has significantly lower math scores on average than the one from 2020-2019 (pre-COVID).

Function's output:
```
> lower_mean_t_test(avg_2023,n_2023,S_2023, avg_2020,n_2020,S_2020, 0.05)
t_stat: -126.7547 , v: 3982576 , cutoff -1.644854 , pvalue: 0 
[1] "reject the null, mean x is significantlly smaller than mean y"
```

## Test results and conclusion.

All of the two-sample t-tests rejected the null hypothesis in favor of the alternative, meaning that, for the three most recent  school cycles, the average math SAT scores obtained by high school students who want to attend college in the near future during each of those school cycles are significantly lower than the scores prior to the COVID-19 pandemic. As I mentioned at the beginning, these tests do not prove any sort of causality, so we can’t say anything about the theories that I started this analysis with. Instead, the value of this analysis lies in the fact that we now know something changed between the pre-COVID and post-COVID times to cause such an effect. Knowing this, we can then look for data or perhaps start experiments that create the necessary data to test different theories. 

### If you are wondering about the during-COVID school cycles

Lastly, I want to explain why I left the during-COVID school cycles out of this analysis. The COVID time is not similar to either the post-COVID time or the pre-COVID time, and the main problem with this is that a lot of the assumptions I comfortably made about the pre-COVID and post-COVID times, I just can’t make about the COVID time. Without these assumptions, I need a completely different approach to study the data from those years. Originally, I planned to write another section here about the data from the COVID years, but since this analysis is already longer than expected, I’ll write about the data from the COVID years in a different post. There’s a very interesting shift in these years, and I encourage you to look for that post on this website in case it's already published. Thank you for reading this post.