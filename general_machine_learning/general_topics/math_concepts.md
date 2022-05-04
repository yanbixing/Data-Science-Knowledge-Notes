### Math Concept

#### Frequentist probability vs Bayesian probability

- Frequentist interpretation: "Probability" is the limit of frequency in <font color="#0000dd">**many trials**</font>.
  - Suitable for events that is repeatable: you can do experiment many times.
    - The probability of head up for a coin. (You can throw 100 times.)
- Bayesian interpretation: "Probability" is Reasonable expectation representing **knowledge/belief**.
  - Suitable for not-repeatable events.
    - The chance of rain of today is 30%. (Today will not repeat many times. It is a inference/belief, not experimental results.)
    - Doctor say you have 40% chance to get cold.
  - Ref [Flower Book P55](), [Wiki-Bayesian_probability](https://en.wikipedia.org/wiki/Bayesian_probability), [Wiki-Frequentist_probability](https://en.wikipedia.org/wiki/Frequentist_probability)

#### Gradient vs Derivative

- Gradient: A vector, whose value on each axis takes the corresponding. 
  $$\nabla f(x) = (\partial_x f, \partial_y f, \partial_z f)$$
  - The gradient is perpendicular to the contour line.
  - The direction of the steepest slope or grade.
- Derivative: A scaler. The change speed of the function along a a specific axis/direction. Also called directional derivative.
  $$\partial_\mu f = \frac{\Delta f}{ \Delta x} = \nabla f \cdot \vec{1}_\mu$$
- Ref: [Oregon State U](https://math.oregonstate.edu/home/programs/undergrad/CalculusQuestStudyGuides/vcalc/grad/grad.html), [Wiki-Gradient](https://en.wikipedia.org/wiki/Gradient), [Wiki-Derivative](https://en.wikipedia.org/wiki/Derivative)