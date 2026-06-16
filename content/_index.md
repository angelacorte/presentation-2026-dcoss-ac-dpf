+++

title = "Guide for writing markdown slides"
description = "A Hugo theme for creating Reveal.js presentations"
outputs = ["Reveal"]

+++

# Title

[**Angela Cortecchia**](mailto:angela.cortecchia@unibo.it)

<div style="text-align: center; width: 100%;">
<img src="example-background.svg" style="width: 40%" />
</div>

--- 

{{< slide background-image="./images/smartstation.png" background-opacity="0.55">}}

{{< rectdiv style="max-width: 90%;" >}}

# Base<small>1</small>

{{% multicol %}}

{{% col %}}

### first col

<ul>
    <li class="fragment" data-fragment-index=0>who</li>
    <li class="fragment" data-fragment-index=1>are</li>
    <li class="fragment" data-fragment-index=2>them</li>
</ul>
{{% /col %}}

{{% col %}}
### second col 

<ul>
    <li class="fragment" data-fragment-index=0>pippo</li>
    <li class="fragment" data-fragment-index=1>pluto</li>
    <li class="fragment" data-fragment-index=2>paperino</li>
</ul>

{{% /col %}}

{{% /multicol %}}


<div>
<small style="text-align: left">
</br>
[1] Beal, J., Pianini, D., Viroli, M. "Aggregate Programming for the Internet of Things." 2015.</br>
</small>
</div>

{{< /rectdiv >}}

