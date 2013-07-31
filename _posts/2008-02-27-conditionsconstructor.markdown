--- 
wordpress_id: 85
title: ConditionsConstructor
date: 2008-02-27 08:04:59 +00:00
wordpress_url: http://blog.footle.org/2008/02/27/conditionsconstructor/
layout: post
---
I hadn't posted this before because it seemed kind of trivial and silly, but I used it again the other day and decided maybe I should share the joy after all.

Far too often I find myself jumping through hoops building ActiveRecord <code>find</code> conditions arrays dynamically depending on what arguments were passed in to the method. So I cooked up a simple little class to clean that up a bit:

{% highlight ruby %}
# class to construct conditions for AR queries
# cc = ConditionsConstructor.new('foobar &gt; ?', 7)
# cc.add('blah is not null')
# cc.add('baz in (?)', [1,2,3])
# cc.conditions
# # => ["foobar &gt; ? and blah is not null and baz in (?)", 7, [1, 2, 3]]
class ConditionsConstructor
  def initialize(str = nil, *args)
    @conditions_strs = str ? [str] : []
    @conditions_args = args
  end
  
  def add(str, *args)
    @conditions_strs &lt;&lt; str
    @conditions_args += args
  end
  
  def conditions
    [@conditions_strs.join(' and ')] + @conditions_args
  end
end
{% endhighlight %}

