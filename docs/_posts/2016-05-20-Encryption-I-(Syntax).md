---
layout: post
---

An encryption function is a way to transform a sequence of characters into another sequence of characters. We introduce some notation to formalize this a little bit. Let $\mathcal{M}$ be the set of all sequences of characters that you care about (for example, $\mathcal{M}$ may be the set of all strings of ASCII characters of arbitrary length or the set of all string of zeros and ones of length $256$). These sequences of characters are often called _plaintexts_. Let $\mathcal{C}$ be the set of all encrypted messages, i.e. another set of sequences of characters. These are called _cyphertexts_. Then an encryption function is just a function $e:\mathcal{M}\to\mathcal{C}$ (_e_ stands for "encrypt"). 
To upgrade an encryption function to an encryption _scheme_, one needs also a way to decrypt encoded messages: that is, a function $d:\mathcal{C}\to\mathcal{M}$. The obvious requirement is that the decryption recovers the original message, namely, $d(e(m)) = m$ for all $m\in\mathcal{M}$. Note that this implies that $e$ defines a bijection between $\mathcal{M}$ and $e(\mathcal{M})\subset\mathcal{C}$. 
<!-- For many practical purposes, this is not good: to encode all $n$-bit strings we will need to use also strings of length $\ge n$. So perfect recoverability (i.e. $d\circ e = id$) may be too strong.  -->
Also note that up to now we haven't mentioned anything about security: after all, we can take $\mathcal{M}=\mathcal{C}$, $e=d=id$ in the definitions above.\
In the final definition, the functions will actually be algorithms (we want the encryption to be computable), of polynomial-time complexity (efficiently computable). For technical reasons, they will be probabilistic algorithms. For a formal definition of probabilistic algorithms see [wikipedia][wiki] - basically it is a Turing machine that at each state tosses a coin to determine which of two transition functions it follows.


**Keys.**  Assume we have two PPT (probabilistic polynomial-time) algorithms $\mathtt{Encode}$ and $\mathtt{Decode}$, corresponding to the functions above. In modern cryptography, we require that all algorithms be publicly known [^1]. In view of this, these two algorithms do not suffice to provide a secure scheme if a sender wants to send a secret to a receiver over an insecure channel (insecure channel means that the cyphertext can be intercepted by an attacker): the attacker would simply run $\mathtt{Decode}$ on the cyphertext and recover the secret. There must be some data that distinguishes the receiver by any other entity: this piece of data is called key. We denote the set of keys by $\mathcal{K}$ (again some set of sequences of characters).

**Security parameter.** Thus, the third component of an encryption scheme will be a PPT algorithm that generates a key or keys. This component is common to practically every protocol in cryptography. The key-generation algorithm takes as input a positive integer $\lambda$, called _security parameter_, in the form of a string of ones of length $\lambda$, denoted by $1^\lambda$. Most of the time this integer is correlated to the "complexity" of the key, for example its length.


**The formal definition.** A private-key encryption scheme is a triple $(\mathtt{GenerateKey, Encode, Decode})$ of PPT algorithms such that:
1. $\mathtt{GenerateKey}(1^\lambda) = sk\in \mathcal{K}$. Here $\lambda\in\mathbb{N}$ is the security parameter and sk is the secret key.
2. $\mathtt{Encode}$ and $\mathtt{Decode}$ have inputs in $\mathcal{K}\times\mathcal{M}$ and outputs in $\mathcal{C}$.
3. For all $sk$ in the target of $\mathtt{GenerateKey}$ and for all $m\in\mathcal{M}$, \\[P[ \mathtt{Decode}(sk,\mathtt{Encode}(sk,m)) = m ]=1,\\] where the probability is taken over the randomness in the algorithms $\mathtt{Encode}$ and $\mathtt{Decode}$.

[wiki]: https://en.wikipedia.org/wiki/Probabilistic_Turing_machine
[^1]: Some reasons why this is useful:?.
<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works. -->

<!-- Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
