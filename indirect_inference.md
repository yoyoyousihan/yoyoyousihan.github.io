<!DOCTYPE HTML>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <meta name="Keywords" content="blog"/>
    <meta name="Description" content="blog"/>
    <title>Simple</title>
    <link rel="shortcut icon" href="/static/favicon.png"/>
    <link rel="stylesheet" type="text/css" href="/main.css" />
</head>
<body>
<div class="main">
    <div class="header">
    	<ul id="pages">
            <li><a href="/">home</a></li>
            <li><a href="/#/tags">tags</a></li>
            <li><a href="/#/archive">archive</a></li>
    	</ul>
    </div>
	<div class="wrap-header">
	<h1>
    <a href="/" id="title"></a>
	</h1>
	</div>
<div id="md" style="display: none;">
<!-- markdown -->
##  <font color=DodgerBlue>1.  What is Indirect Inference Method?</font> 

We've just seen an example in the last post that sometimes, `Generated Method of Moment ` and `Maximum Likelihood Estimation` don't work when it's difficult to find an explicit expression of target function. 

`Simulated Method of Moment` uses a series of simulated pseudo data to minimise the distance between the moment of pseudo data and that of real data. However, sometimes, even the moments are not promising enough especially when we search a specific property. Then, we want to generalise the moment estimation to statistics estimation, in this case, we can choose  `Indirect Inference` method to estimate the parameters.

In fact, `Simulated Method of Moment` is a special case of `Indirect Inference`Method when the statistics are the moments of data.

##  <font color=DodgerBlue>2.  Indirect Inference Method</font> 

First, given an initial set of parameters $  \theta \in \Theta \subset \mathbb{R}^d  $ we simulate a series of data $ \\{y _i(\theta)\\} _{i =1,2,...,N} $ following the model $F(\theta)$.

Then, we want to find an auxiliary model $G(\mu)$, where $\mu \in U \subset \mathbb{R}^r $ where $r\geq d$, so that a direct method of estimation is feasible, supposing the target function to be maximised is  $Q(\mu)$ then:

$$
\tilde{\mu} _ G = \underset{\mu\in \mathbb{R}^r }{arg max} \ \ Q(y; \mu)
$$

For the real data set $\\{y _t\\}  _{t =1,2,...,T}$, we can easily compute $\tilde\mu _G$:

$$
\tilde{\mu} _ G = \underset{\mu\in \mathbb{R}^r }{arg max}\ \  Q(  \\{ y _t\\} _ {t= 1,2,...T}; \mu)
$$

The same for the simulated pseudo data:
$$
\tilde{\mu} _ G(\theta) = \underset{\mu\in \mathbb{R}^r }{arg max}\ \  Q(  \\{ y _i(\theta)\\} _ {i= 1,2,...N}; \mu)
$$

Our problem becomes:

$$
\hat{\theta} = \underset { \theta \in \mathbb{R}^d}{arg min} [\tilde{\mu} _ G(\theta)-\tilde{\mu} _ G]^T \Omega  [\tilde{\mu} _ G(\theta)-\tilde{\mu} _ G]
$$

where $ \Omega$ is the symmetric positive definite weight matrix. 

##  <font color=DodgerBlue>3. Use in Stochastic Volatility model</font> 

Our model to be estimated is: 

$$
y _t = e^{h _t/2} u _t
$$
$$
h _t = a + b h _{t-1} + \sigma \epsilon _t
$$
where:
$$
\mathcal{N}(0,1)\sim u _t \perp \epsilon _t \sim \mathcal{N}(0,1)
$$

and

$$
h_1 \sim \mathcal{N}(\frac{a}{1-b},\frac{\sigma^2}{1-b^2})
$$

so : $\theta = (a, b, \sigma) \in \mathbb{R} \times (-1,1) \times  \mathbb{R} ^*$

We choose an auxiliary model $G(\mu)$:

$$
ln(y _t^2) = \mu _1 ln(y _{t-1}^2)+ \mu _2 + \mu _3 e _t 
$$

where $e_t$ is the residual of the model and satisfies $e _t \sim i.i.d. \mathcal{N}(0,1)$ , and $\mu = (\mu _1, \mu _2, \mu _3) \in (-1,1) \times \mathbb{R} \times \mathbb{R}^*$.

$\hat{\mu} $can be the ML estimators of $G(\mu)$ model. 
<!-- markdown end -->
</div>
<div class="entry" id="main">
<!-- content -->

<!-- content end -->
</div>
<br>
<br>
    <div id="disqus_thread"></div>
	<div class="footer">
		<p>© Copyright 2014 by isnowfy, Designed by isnowfy</p>
	</div>
</div>
<script src="main.js"></script>
<script id="content" type="text/mustache">
    <h1>{{title}}</h1>
    <div class="tag">
    {{date}}
    {{#tags}}
    <a href="/#/tag/{{name}}">#{{name}}</a>
    {{/tags}}
    </div>
</script>
<script id="pagesTemplate" type="text/mustache">
    {{#pages}}
    <li>
        <a href="{{path}}">{{title}}</a>
    </li>
    {{/pages}}
</script>
<script>
$(document).ready(function() {
    $.ajax({
        url: "main.json",
        type: "GET",
        dataType: "json",
        success: function(data) {
            $("#title").html(data.name);
            var pagesTemplate = Hogan.compile($("#pagesTemplate").html());
            var pagesHtml = pagesTemplate.render({"pages": data.pages});
            $("#pages").append(pagesHtml);
            //path
            var path = "indirect_inference.md";
            //path end
            var now = 0;
            for (var i = 0; i < data.posts.length; ++i)
                if (path == data.posts[i].path)
                    now = i;
            var post = data.posts[now];
            var tmp = post.tags.split(" ");
            var tags = [];
            for (var i = 0; i < tmp.length; ++i)
                if (tmp[i].length > 0)
                    tags.push({"name": tmp[i]});
            var contentTemplate = Hogan.compile($("#content").html());
            var contentHtml = contentTemplate.render({"title": post.title, "tags": tags, "date": post.date});
            $("#main").prepend(contentHtml);
            if (data.disqus_shortname.length > 0) {
                var disqus_shortname = data.disqus_shortname;
                (function() {
                    var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
                    dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
                    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
                })();
            }
        }
    });
});
</script>
<script src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ["\\(", "\\)"]], processEscapes: true}});
</script>
</body>
</html>
