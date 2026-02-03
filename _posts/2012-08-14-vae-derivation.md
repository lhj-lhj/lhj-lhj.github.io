---
title: "Variational Autoencoder (VAE)"
date: 2012-08-14
permalink: /posts/2012/08/vae-derivation/
tags:
  - cool posts
  - category1
  - category2
---

## 1. Problem Setup: Latent Variable Model

We assume that real-world observations $x \in \mathcal{X}$ are generated from latent variables $z \in \mathbb{R}^d$ through an underlying and unknown generative process. Conceptually, this process can be viewed as a compression of the true world state into a structured latent representation, followed by a decoding step that produces observable data:

$$
\omega
\xrightarrow{\text{information bottleneck}}
z
\xrightarrow{\text{decoder}}
x.
$$

Here, $\omega$ denotes the (unobservable) state of the real world, while $z$ represents a learnable latent variable that serves as a structured and low-dimensional abstraction sufficient to explain the observed data.

Our goal is to learn a meaningful latent representation $z$ that captures the dominant generative factors of the data, and to construct a generative model that enables sampling in the latent space and the generation of new observations $\hat{x}$ consistent with real-world constraints.

---

### 2. Data Distribution and Learning Objective

Let $X$ be a random variable taking values in $\mathcal{X}$, with an unknown data-generating distribution $p_X$. We are given a dataset consisting of $N$ independent and identically distributed observations:

$$
x^{(1)}, \dots, x^{(N)} \overset{\text{i.i.d.}}{\sim} p_X.
$$

We aim to learn the parameters $\theta$ of a generative model that induces a model distribution $p_{X_\theta}$ (with density $p_\theta(x)$) approximating the true data distribution $p_X$. This is achieved by **maximum likelihood estimation**, i.e., by maximizing the likelihood of the observed data under the model.

Formally, the learning objective is given by:

$$
\theta^*
=
\arg\max_{\theta}
\sum_{i=1}^{N}
\log p_\theta\big(x^{(i)}\big).
$$

We denote the resulting learned distribution as $\hat{p}_X$, which serves as an empirical approximation of the true data-generating distribution.

But computing $p_\theta(x)$ requires the integral

$$
p_\theta(x) = \int p_\theta(x, z)\, dz = \int p(z)\, p_\theta(x \mid z)\, dz,
$$

which is usually **intractable** for expressive decoders (e.g., neural networks), because the integral is high-dimensional and has no closed form. 

## 3. Introduce an Approximate Posterior (Encoder)

The true posterior is

$$
p_\theta(z \mid x) = \frac{p_\theta(x, z)}{p_\theta(x)} = \frac{p(z)\, p_\theta(x \mid z)}{\int p(z)\, p_\theta(x \mid z)\, dz}.
$$

This is also intractable. VAEs introduce an approximate posterior

$$
q_\phi(z \mid x),
$$

parameterized by $\phi$ (the **encoder**), intended to approximate $p_\theta(z \mid x)$.


## 4. Deriving the ELBO (Evidence Lower Bound)

Start from the log evidence:

$$
\log p_\theta(x).
$$

Insert $q_\phi(z \mid x)$ via an expectation:

$$
\log p_\theta(x)
= \log \int p_\theta(x, z)\, dz
= \log \int q_\phi(z \mid x)\frac{p_\theta(x, z)}{q_\phi(z \mid x)}\, dz.
$$

Rewrite as an expectation:

$$
\log p_\theta(x)
= \log \mathbb{E}_{q_\phi(z \mid x)}\left[\frac{p_\theta(x, z)}{q_\phi(z \mid x)}\right].
$$

Apply **Jensen’s inequality** ($\log \mathbb{E}[Y] \ge \mathbb{E}[\log Y]$):

$$
\log p_\theta(x)
\ge
\mathbb{E}_{q_\phi(z \mid x)}\left[\log \frac{p_\theta(x, z)}{q_\phi(z \mid x)}\right].
$$

Define the **ELBO**:

$$
\mathcal{L}(\theta,\phi; x)
=
\mathbb{E}_{q_\phi(z \mid x)}\left[\log p_\theta(x, z) - \log q_\phi(z \mid x)\right].
$$

So we have:

$$
\log p_\theta(x) \ge \mathcal{L}(\theta,\phi; x).
$$

## 5. Expand the ELBO into the Standard Form

Use $p_\theta(x, z)=p(z)p_\theta(x \mid z)$:

$$
\mathcal{L}(\theta,\phi; x)
=
\mathbb{E}_{q_\phi(z \mid x)}\left[\log p(z) + \log p_\theta(x \mid z) - \log q_\phi(z \mid x)\right].
$$

Group terms:

$$
\mathcal{L}(\theta,\phi; x)
=
\mathbb{E}_{q_\phi(z \mid x)}[\log p_\theta(x \mid z)]
+
\mathbb{E}_{q_\phi(z \mid x)}[\log p(z) - \log q_\phi(z \mid x)].
$$

Recognize the [KL divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence):

$$
\mathrm{KL}(q_\phi(z \mid x) \| p(z))
=
\mathbb{E}_{q_\phi(z \mid x)}\left[\log \frac{q_\phi(z \mid x)}{p(z)}\right]
=
\mathbb{E}_{q_\phi(z \mid x)}[\log q_\phi(z \mid x) - \log p(z)].
$$

Thus:

$$
\mathbb{E}_{q_\phi(z \mid x)}[\log p(z) - \log q_\phi(z \mid x)]
=
-\mathrm{KL}(q_\phi(z \mid x) \| p(z)).
$$

So the ELBO becomes:

$$
\boxed{
\mathcal{L}(\theta,\phi; x)
=
\mathbb{E}_{q_\phi(z \mid x)}[\log p_\theta(x \mid z)]
-
\mathrm{KL}(q_\phi(z \mid x) \| p(z))
}
$$

## 6. Relationship Between ELBO and the True Posterior

**Note that maximizing the objective derived from Jensen’s inequality is not equivalent to maximizing the true likelihood.**

We can make the exact nature of the “gap” explicit. Start with:

$$
\mathrm{KL}(q_\phi(z \mid x) \| p_\theta(z \mid x))
=
\mathbb{E}_{q_\phi(z \mid x)}\left[\log \frac{q_\phi(z \mid x)}{p_\theta(z \mid x)}\right].
$$

Plug in the definition of the true posterior:

$$
p_\theta(z \mid x) = \frac{p_\theta(x, z)}{p_\theta(x)}.
$$

Then:

$$
\log \frac{q_\phi(z \mid x)}{p_\theta(z \mid x)}
= \log q_\phi(z \mid x) - \log p_\theta(x, z)
+
\log p_\theta(x).
$$

Taking the expectation with respect to $q_\phi(z \mid x)$ yields:

$$
\mathrm{KL}(q_\phi(z \mid x) \| p_\theta(z \mid x))
=
\mathbb{E}_{q_\phi(z \mid x)}\left[\log q_\phi(z \mid x) - \log p_\theta(x, z)\right]
+
\log p_\theta(x).
$$

Rearranging terms, we obtain:

$$
\log p_\theta(x)
=
\mathbb{E}_{q_\phi(z \mid x)}\left[\log p_\theta(x, z) - \log q_\phi(z \mid x)\right]
+
\mathrm{KL}(q_\phi(z \mid x) \| p_\theta(z \mid x)).
$$

The first term corresponds exactly to the ELBO. Hence,

$$
\boxed{
\log p_\theta(x) = \mathcal{L}(\theta,\phi; x)
+
\mathrm{KL}(q_\phi(z \mid x) \| p_\theta(z \mid x))
}
$$

Therefore, the ELBO and the true log-likelihood always differ by a KL divergence term.

Since KL divergence is nonnegative, the ELBO is indeed a lower bound on $\log p_\theta(x)$.
Furthermore, **for fixed $\theta$**, maximizing the ELBO with respect to $\phi$ is equivalent to minimizing the variational gap
$\mathrm{KL}\left(q_\phi(z \mid x) \| p_\theta(z \mid x)\right)$, thereby improving the posterior approximation **within the chosen variational family**.
In practice, VAEs optimize $\theta$ and $\phi$ jointly via stochastic gradient ascent on the ELBO; this differs fundamentally from classical EM and does **not** guarantee that the variational gap is minimized at each update.

## 7. Practical Choice: Gaussian Encoder and Standard Normal Prior

A common VAE choice:

- Prior: $p(z) = \mathcal{N}(0, I)$
- Approx posterior: $q_\phi(z \mid x) = \mathcal{N}(\mu_\phi(x), \mathrm{diag}(\sigma_\phi^2(x)))$

So the encoder outputs $\mu_\phi(x)$ and $\log \sigma_\phi^2(x)$.

### Closed-Form KL Term

For $q=\mathcal{N}(\mu, \mathrm{diag}(\sigma^2))$ and $p=\mathcal{N}(0, I)$, the KL has a closed form:

$$
\boxed{
\mathrm{KL}(q(z \mid x) \| p(z))
=
\frac{1}{2}\sum_{j=1}^d
\left(\sigma_j^2 + \mu_j^2 - 1 - \log \sigma_j^2\right)
}
$$

## 8. Reparameterization Trick (Key Gradient Derivation)

We need to optimize

$$
\mathbb{E}_{q_\phi(z \mid x)}[\log p_\theta(x \mid z)]
$$

with respect to $\phi$. Directly differentiating through a sample $z \sim q_\phi(z \mid x)$ is tricky.

### Reparameterization

If $q_\phi(z \mid x)=\mathcal{N}(\mu_\phi(x), \mathrm{diag}(\sigma_\phi^2(x)))$, sample as:

$$
\epsilon \sim \mathcal{N}(0, I),
\quad
z = \mu_\phi(x) + \sigma_\phi(x) \odot \epsilon.
$$

Now randomness is in $\epsilon$, independent of $\phi$, so:

$$
\mathbb{E}_{q_\phi(z \mid x)}[\log p_\theta(x \mid z)]
=
\mathbb{E}_{\epsilon\sim \mathcal{N}(0, I)}\left[\log p_\theta\left(x \mid \mu_\phi(x) + \sigma_\phi(x) \odot \epsilon\right)\right].
$$

This allows gradients to flow through $\mu_\phi,\sigma_\phi$ using standard backprop.

## 9. Final Training Objective (Loss Form)

We maximize the ELBO, equivalently minimize the negative ELBO:

$$
\max_{\theta,\phi}\ \mathcal{L}(\theta,\phi; x)
\quad\Longleftrightarrow\quad
\min_{\theta,\phi}\ -\mathcal{L}(\theta,\phi; x).
$$

Thus the loss for one sample is:

$$
\boxed{
\mathcal{J}(\theta,\phi; x)
=
-\mathbb{E}_{q_\phi(z \mid x)}[\log p_\theta(x \mid z)]
+
\mathrm{KL}(q_\phi(z \mid x) \| p(z))
}
$$

Interpretation:

- **Reconstruction term**: encourages decoder to reconstruct $x$ from $z$.
- **KL regularizer**: pushes latent distribution toward the prior, enabling sampling and preventing overfitting.

## 10. Likelihood Choices and Reconstruction Loss

The term $\log p_\theta(x \mid z)$ depends on the assumed observation model.

### [Bernoulli](https://en.wikipedia.org/wiki/Bernoulli_distribution) Likelihood (e.g., normalized binary pixels)

If:

$$
p_\theta(x \mid z)=\mathrm{Bernoulli}(x;\pi_\theta(z)),
$$

then:

$$
-\log p_\theta(x \mid z) = \text{binary cross entropy}(x, \pi_\theta(z)).
$$

### Gaussian Likelihood (real-valued data)

If:

$$
p_\theta(x \mid z)=\mathcal{N}(x; \mu_\theta(z), \sigma^2 I),
$$

then (up to constants):

$$
-\log p_\theta(x \mid z) \propto \frac{1}{2\sigma^2}\|x-\mu_\theta(z)\|^2,
$$

so reconstruction becomes [MSE](https://en.wikipedia.org/wiki/Mean_squared_error) (scaled).
