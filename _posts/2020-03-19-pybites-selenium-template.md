---
keywords: fastai
description: "Resolver y realizar Test para PyBites usando Selenium"
title: PyBites Selenium Template for Exercises
toc: false
branch: master
badges: true
comments: true
categories: [selenium, python, pytest, jupyter, template, pybites]
nb_path: _notebooks/2020-03-19-pybites-selenium-template.ipynb
layout: notebook
---

<!--
#################################################
### THIS FILE WAS AUTOGENERATED! DO NOT EDIT! ###
#################################################
# file to edit: _notebooks/2020-03-19-pybites-selenium-template.ipynb
-->

<div class="container" id="notebook-container">
        
<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h1 id="Intro">Intro<a class="anchor-link" href="#Intro"> </a></h1><p>Este notebook se podrá usar como plantilla para resolver los ejercicios <a href="https://codechalleng.es/bites/">PyBites</a> de la plataforma <a href="https://codechalleng.es/">codechalleng.es</a></p>
<h2 id="Pre-requerimientos">Pre-requerimientos<a class="anchor-link" href="#Pre-requerimientos"> </a></h2><ul>
<li>Tener una <strong>licencia válida</strong> para poder acceder al ejercicio (Bite) completo.</li>
<li>Intalar <strong>chromium-chromedriver</strong> y configurarlo.</li>
<li>Instalar Selenium.</li>
</ul>
<h2 id="Uso">Uso<a class="anchor-link" href="#Uso"> </a></h2><ul>
<li>Establecer como variable de entorno del sistema global tu <strong>USERNAME</strong> y <strong>PASSWORD</strong> de la plataforma codechalleng.es <strong>(Celda #1)</strong></li>
<li>Realizar el <strong>proceso de Login en la plataforma codechalleg.es</strong> ejecutando la <strong>(Celda #2)</strong></li>
<li>Establecer el número del ejercicio <strong>(BITE_NUM)</strong> a resolver <strong>(Celda #3)</strong></li>
<li>Mostrar el contenido del archivo <strong>PB_FILE</strong> utilizando el comando <strong>!cat</strong> <strong>(Celda #4)</strong></li>
<li>Imprimir el nombre del archivo <strong>(BITE_NAME)</strong> <strong>(Celda #5)</strong></li>
<li>Sobrescribir el archivo <strong>(BITE_NAME)</strong> con la solución utilizando el comando mágico de celda <strong>%%writefile</strong> <strong>(Celda #6)</strong></li>
<li>Asegurarse de pasar las pruebas <strong>(PB_TEST)</strong> utilizando <strong>pytest</strong> <strong>(Celda #7)</strong></li>
<li><strong>Salvar la solución (CODE)</strong> y ejecutar las pruebas en la plataforma codechalleng.es <strong>(Celda #8)</strong></li>
<li>Resolver otros ejercicios, <strong>repetir el proceso a partir de la* </strong>(Celda #3)**</li>
</ul>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># https://gist.github.com/korakot/5c8e21a5af63966d80a676af0ce15067</span>
<span class="o">!</span>apt install chromium-chromedriver
<span class="o">!</span>pip install selenium

<span class="o">%</span><span class="k">env</span> PB_USER=YOUR_USERNAME
<span class="o">%</span><span class="k">env</span> PB_PW=YOUR_PASSWORD
</pre></div>

    </div>
</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># https://gist.github.com/pybites/0aa6d9833849a0942ed218b1d46c47b4#file-platform_login_selenium-py</span>
<span class="kn">import</span> <span class="nn">os</span>
<span class="kn">from</span> <span class="nn">IPython.display</span> <span class="kn">import</span> <span class="n">HTML</span>
<span class="c1"># make venv and pip install selenium</span>
<span class="kn">from</span> <span class="nn">selenium</span> <span class="kn">import</span> <span class="n">webdriver</span>
<span class="kn">from</span> <span class="nn">selenium.webdriver.common.by</span> <span class="kn">import</span> <span class="n">By</span>
<span class="kn">from</span> <span class="nn">selenium.webdriver.common.action_chains</span> <span class="kn">import</span> <span class="n">ActionChains</span>
<span class="kn">from</span> <span class="nn">selenium.webdriver.common.keys</span> <span class="kn">import</span> <span class="n">Keys</span>
<span class="kn">from</span> <span class="nn">selenium.webdriver.support.ui</span> <span class="kn">import</span> <span class="n">WebDriverWait</span>
<span class="kn">from</span> <span class="nn">selenium.webdriver.support</span> <span class="kn">import</span> <span class="n">expected_conditions</span> <span class="k">as</span> <span class="n">EC</span>


<span class="c1"># Set your codechalleng.es username and password in venv/bin/activate, then source it</span>
<span class="n">USERNAME</span> <span class="o">=</span> <span class="n">os</span><span class="o">.</span><span class="n">getenv</span><span class="p">(</span><span class="s2">&quot;PB_USER&quot;</span><span class="p">)</span> <span class="ow">or</span> <span class="s2">&quot;YOUR_USERNAME&quot;</span>
<span class="n">PASSWORD</span> <span class="o">=</span> <span class="n">os</span><span class="o">.</span><span class="n">getenv</span><span class="p">(</span><span class="s2">&quot;PB_PW&quot;</span><span class="p">)</span> <span class="ow">or</span> <span class="s2">&quot;YOUR_PASSWORD&quot;</span>

<span class="n">BASE_URL</span> <span class="o">=</span> <span class="s2">&quot;https://codechalleng.es&quot;</span>
<span class="n">LOGIN_URL</span> <span class="o">=</span> <span class="sa">f</span><span class="s2">&quot;</span><span class="si">{</span><span class="n">BASE_URL</span><span class="si">}</span><span class="s2">/login/&quot;</span>
<span class="n">WAIT_SECONDS</span> <span class="o">=</span> <span class="mi">2</span>
<span class="n">CODE_TEMPLATE</span> <span class="o">=</span> <span class="s2">&quot;&quot;</span>
<span class="n">TEST_NAME</span> <span class="o">=</span> <span class="s2">&quot;&quot;</span>
<span class="n">CODE_TEST</span> <span class="o">=</span> <span class="s2">&quot;&quot;</span>


<span class="c1"># Set options to be headless, ..</span>
<span class="n">options</span> <span class="o">=</span> <span class="n">webdriver</span><span class="o">.</span><span class="n">ChromeOptions</span><span class="p">()</span>
<span class="n">options</span><span class="o">.</span><span class="n">add_argument</span><span class="p">(</span><span class="s1">&#39;--headless&#39;</span><span class="p">)</span>
<span class="n">options</span><span class="o">.</span><span class="n">add_argument</span><span class="p">(</span><span class="s1">&#39;--no-sandbox&#39;</span><span class="p">)</span>
<span class="n">options</span><span class="o">.</span><span class="n">add_argument</span><span class="p">(</span><span class="s1">&#39;--disable-dev-shm-usage&#39;</span><span class="p">)</span>


<span class="c1"># Open it, go to a website, and get results</span>
<span class="n">driver</span> <span class="o">=</span> <span class="n">webdriver</span><span class="o">.</span><span class="n">Chrome</span><span class="p">(</span><span class="s1">&#39;chromedriver&#39;</span><span class="p">,</span> <span class="n">options</span><span class="o">=</span><span class="n">options</span><span class="p">)</span>

<span class="n">driver</span><span class="o">.</span><span class="n">get</span><span class="p">(</span><span class="n">LOGIN_URL</span><span class="p">)</span>
  
<span class="n">driver</span><span class="o">.</span><span class="n">find_element_by_name</span><span class="p">(</span><span class="s1">&#39;username&#39;</span><span class="p">)</span><span class="o">.</span><span class="n">send_keys</span><span class="p">(</span><span class="n">USERNAME</span><span class="p">)</span>
<span class="n">driver</span><span class="o">.</span><span class="n">find_element_by_name</span><span class="p">(</span><span class="s1">&#39;password&#39;</span><span class="p">)</span><span class="o">.</span><span class="n">send_keys</span><span class="p">(</span><span class="n">PASSWORD</span> <span class="o">+</span> <span class="n">Keys</span><span class="o">.</span><span class="n">RETURN</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;You are being logged&quot;</span><span class="p">)</span>
  
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>You are being logged
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Set your Bite to code</span>
<span class="n">BITE_NUM</span> <span class="o">=</span> <span class="mi">110</span>

<span class="n">PYBITES_BITE_URL</span> <span class="o">=</span> <span class="sa">f</span><span class="s2">&quot;</span><span class="si">{</span><span class="n">BASE_URL</span><span class="si">}</span><span class="s2">/bites/</span><span class="si">{</span><span class="n">BITE_NUM</span><span class="si">}</span><span class="s2">&quot;</span>
<span class="n">driver</span><span class="o">.</span><span class="n">get</span><span class="p">(</span><span class="n">PYBITES_BITE_URL</span><span class="p">)</span>
<span class="c1"># do your thing</span>
<span class="c1">#_html = driver.page_source</span>
<span class="c1">#print(_html)</span>
<span class="n">contentWrapperElement</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">find_element_by_css_selector</span><span class="p">(</span><span class="s2">&quot;div.mui-col-md-8&quot;</span><span class="p">)</span>
<span class="n">contentWrapperElement</span> <span class="o">=</span> <span class="n">contentWrapperElement</span><span class="o">.</span><span class="n">get_attribute</span><span class="p">(</span><span class="s2">&quot;innerHTML&quot;</span><span class="p">)</span>
<span class="c1"># Implicit Wait</span>
<span class="n">driver</span><span class="o">.</span><span class="n">implicitly_wait</span><span class="p">(</span><span class="n">WAIT_SECONDS</span><span class="p">)</span>
<span class="n">element</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">find_element_by_name</span><span class="p">(</span><span class="s2">&quot;template_code&quot;</span><span class="p">)</span>
<span class="n">CODE_TEMPLATE</span> <span class="o">=</span> <span class="n">element</span><span class="o">.</span><span class="n">get_attribute</span><span class="p">(</span><span class="s2">&quot;value&quot;</span><span class="p">)</span>
<span class="n">element</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">find_element_by_xpath</span><span class="p">(</span><span class="s2">&quot;//a[@data-mui-controls=&#39;pane-default-2&#39;]/span&quot;</span><span class="p">)</span>
<span class="n">element</span><span class="o">.</span><span class="n">click</span><span class="p">()</span>
<span class="n">TEST_NAME</span> <span class="o">=</span> <span class="n">element</span><span class="o">.</span><span class="n">text</span>
<span class="n">TEST_NAME</span> <span class="o">=</span> <span class="n">TEST_NAME</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s2">&quot;(&quot;</span><span class="p">)</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s2">&quot;)&quot;</span><span class="p">)</span>
<span class="n">driver</span><span class="o">.</span><span class="n">execute_script</span><span class="p">(</span><span class="s2">&quot;mui.tabs.activate(&#39;pane-default-2&#39;)&quot;</span><span class="p">)</span>
<span class="c1"># Simular Copy/Paste en Selenium</span>
<span class="c1"># Copy</span>
<span class="n">cb</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">execute_script</span><span class="p">(</span><span class="s2">&quot;return copyTestToClipBoard();&quot;</span><span class="p">)</span>
<span class="n">alert</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">switch_to</span><span class="o">.</span><span class="n">alert</span>
<span class="n">alert</span><span class="o">.</span><span class="n">accept</span><span class="p">()</span>
<span class="n">driver</span><span class="o">.</span><span class="n">switch_to</span><span class="o">.</span><span class="n">default_content</span>
<span class="c1"># Crear textArea</span>
<span class="n">js</span> <span class="o">=</span> <span class="s2">&quot;&quot;&quot;</span>
<span class="s2">function createTextArea(){</span>
<span class="s2">      var tArea = document.createElement(&#39;textarea&#39;);</span>
<span class="s2">      tArea.id = &#39;myTestCode&#39;;</span>
<span class="s2">      var pane = document.getElementById(&#39;pane-default-2&#39;);</span>
<span class="s2">      pane.appendChild(tArea);</span>
<span class="s2">}</span>
<span class="s2">createTextArea();</span>
<span class="s2">&quot;&quot;&quot;</span>
<span class="n">cb</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">execute_script</span><span class="p">(</span><span class="n">js</span><span class="p">)</span>
<span class="c1"># Paste</span>
<span class="n">textAreaElement</span> <span class="o">=</span> <span class="n">driver</span><span class="o">.</span><span class="n">find_element_by_id</span><span class="p">(</span><span class="s2">&quot;myTestCode&quot;</span><span class="p">)</span>
<span class="n">textAreaElement</span><span class="o">.</span><span class="n">click</span><span class="p">()</span>
<span class="n">textAreaElement</span><span class="o">.</span><span class="n">send_keys</span><span class="p">(</span><span class="n">Keys</span><span class="o">.</span><span class="n">CONTROL</span> <span class="o">+</span> <span class="s2">&quot;v&quot;</span><span class="p">)</span>
<span class="n">CODE_TEST</span> <span class="o">=</span> <span class="n">textAreaElement</span><span class="o">.</span><span class="n">get_property</span><span class="p">(</span><span class="s2">&quot;value&quot;</span><span class="p">)</span>


<span class="n">BITE_NAME</span> <span class="o">=</span> <span class="n">TEST_NAME</span><span class="o">.</span><span class="n">split</span><span class="p">(</span><span class="s2">&quot;test_&quot;</span><span class="p">)[</span><span class="mi">1</span><span class="p">]</span>

<span class="k">with</span> <span class="nb">open</span><span class="p">(</span><span class="n">BITE_NAME</span><span class="p">,</span> <span class="s2">&quot;w&quot;</span><span class="p">)</span> <span class="k">as</span> <span class="n">file</span><span class="p">:</span>
  <span class="n">file</span><span class="o">.</span><span class="n">write</span><span class="p">(</span><span class="n">CODE_TEMPLATE</span><span class="p">)</span>
  <span class="n">os</span><span class="o">.</span><span class="n">environ</span><span class="p">[</span><span class="s2">&quot;PB_FILE&quot;</span><span class="p">]</span> <span class="o">=</span> <span class="n">BITE_NAME</span>

<span class="k">with</span> <span class="nb">open</span><span class="p">(</span><span class="n">TEST_NAME</span><span class="p">,</span> <span class="s2">&quot;w&quot;</span><span class="p">)</span> <span class="k">as</span> <span class="n">file</span><span class="p">:</span>
  <span class="n">file</span><span class="o">.</span><span class="n">write</span><span class="p">(</span><span class="n">CODE_TEST</span><span class="p">)</span>
  <span class="n">os</span><span class="o">.</span><span class="n">environ</span><span class="p">[</span><span class="s2">&quot;PB_TEST&quot;</span><span class="p">]</span> <span class="o">=</span> <span class="n">TEST_NAME</span>

<span class="n">_html</span> <span class="o">=</span> <span class="n">contentWrapperElement</span> <span class="o">+</span> <span class="s2">&quot;</span><span class="se">\n\n</span><span class="s2">&quot;</span> <span class="o">+</span> \
<span class="sa">f</span><span class="s2">&quot;&lt;b&gt;</span><span class="si">{</span><span class="n">BITE_NAME</span><span class="si">}</span><span class="s2"> and </span><span class="si">{</span><span class="n">TEST_NAME</span><span class="si">}</span><span class="s2"> files written to local storage!&lt;/b&gt;&quot;</span>

<span class="n">HTML</span><span class="p">(</span><span class="n">_html</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">


<div class="output_html rendered_html output_subarea output_execute_result">

      <h2 class="mui--text-headline title">
      
        <a href="/profiles/pybites" target="_blank">
          <img class="ghMiniAvatar" src="https://github.com/pybites.png?size=40" alt="GH avatar">
        </a> 
      
      Bite 110. Type conversion and exception handling
        

        
          <form id="bitemarkForm" action="" class="mui-form" method="post">
            <input type="hidden" name="csrfmiddlewaretoken" value="40IzwRBKuTaJbaqDtyc3MN8mJ2wQkOTXwrmelKgK0jpDn4KepxpLvGhmnBpH0OXQ">
            <input name="bitemarkChange" id="bitemarkChange" type="hidden" value="1">
            
              <button type="submit" class="bitemarkStar" id="bitemark" name="bitemark" title="Add to your favorites ('bitemark it')">☆</button>
            
          </form>
        
      </h2>

      <blockquote class="biteDescription scrollbar2">
        <p>In this Bite you complete the <code>divide_numbers</code> function that takes a <code>numerator</code> and a <code>denominator</code> (the number above and below the line respectively when doing a division).</p><p>First you try to convert them to <code>int</code>s, if that raises a <code>ValueError</code> you will re-raise it (using <code>raise</code>).</p><p>To keep things simple we can expect this function to be called with  <code>int/float/str </code> types only (read the tests why ...)</p><p>Getting passed that exception (no early bail out, we're still in business) you try to divide <code>numerator</code> by <code>denominator</code> returning its result.</p><p>If <code>denominator</code> is 0 though, Python throws another exception. Figure out which one that is and catch it. In that case return 0.</p>

        

      </blockquote>

      

    

<b>division.py and test_division.py files written to local storage!</b>
</div>

</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">!</span>cat <span class="nv">$PB_FILE</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>def divide_numbers(numerator, denominator):
    &#34;&#34;&#34;For this exercise you can assume numerator and denominator are of type
       int/str/float.
       Try to convert numerator and denominator to int types, if that raises a
       ValueError reraise it. Following do the division and return the result.
       However if denominator is 0 catch the corresponding exception Python
       throws (cannot divide by 0), and return 0&#34;&#34;&#34;
    pass</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Copy and paste the Code Template into the next cell. Write the solution and overwrite it to the file using the magic command <strong>%%writefile \&lt;PB_FILE&gt;</strong> in the first line of the cell</p>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="nb">print</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;%%writefile </span><span class="si">{</span><span class="n">BITE_NAME</span><span class="si">}</span><span class="s2">&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>%%writefile division.py
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%%writefile</span> division.py

<span class="k">def</span> <span class="nf">divide_numbers</span><span class="p">(</span><span class="n">numerator</span><span class="p">,</span> <span class="n">denominator</span><span class="p">):</span>
  <span class="k">try</span><span class="p">:</span>
    <span class="n">numerator</span><span class="p">,</span> <span class="n">denominator</span> <span class="o">=</span> <span class="nb">int</span><span class="p">(</span><span class="n">numerator</span><span class="p">),</span> <span class="nb">int</span><span class="p">(</span><span class="n">denominator</span><span class="p">)</span>
    <span class="n">result</span> <span class="o">=</span> <span class="n">numerator</span> <span class="o">/</span> <span class="n">denominator</span>
  <span class="k">except</span> <span class="ne">ValueError</span> <span class="k">as</span> <span class="n">error</span><span class="p">:</span>
    <span class="k">raise</span><span class="p">(</span><span class="n">error</span><span class="p">)</span>
  <span class="k">except</span> <span class="ne">ZeroDivisionError</span> <span class="k">as</span> <span class="n">error</span><span class="p">:</span>
    <span class="n">result</span> <span class="o">=</span> <span class="mi">0</span>

  <span class="k">return</span> <span class="n">result</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>Overwriting division.py
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Uncomment the next line and run it until passing tests with <strong>pytest</strong></p>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="ch">#!pytest --capture=fd $PB_TEST</span>
<span class="o">!</span>python -m pytest <span class="nv">$PB_TEST</span>
<span class="nb">print</span><span class="p">(</span><span class="sa">f</span><span class="s2">&quot;!python -m pytest -V </span><span class="si">{</span><span class="n">TEST_NAME</span><span class="si">}</span><span class="s2">&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre><span class="ansi-bold">============================= test session starts ==============================</span>
platform linux -- Python 3.6.9, pytest-3.6.4, py-1.8.1, pluggy-0.7.1
rootdir: /content, inifile:
plugins: typeguard-2.7.1
collected 9 items                                                              

test_division.py .........<span class="ansi-cyan-fg">                                               [100%]</span>

<span class="ansi-green-intense-fg ansi-bold">=========================== 9 passed in 0.06 seconds ===========================</span>
!python -m pytest -V test_division.py
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

<div class="cell border-box-sizing text_cell rendered"><div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>When you are sure that your code passes the Test, execute the next cell for <strong>”SAVE AND RUN TESTS</strong></p>

</div>
</div>
</div>
    {% raw %}
    
<div class="cell border-box-sizing code_cell rendered">
<div class="input">

<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">with</span> <span class="nb">open</span><span class="p">(</span><span class="n">BITE_NAME</span><span class="p">,</span> <span class="s2">&quot;rb&quot;</span><span class="p">)</span> <span class="k">as</span> <span class="n">file</span><span class="p">:</span>
  <span class="n">CODE</span> <span class="o">=</span> <span class="n">file</span><span class="o">.</span><span class="n">read</span><span class="p">()</span>

<span class="n">CODE</span> <span class="o">=</span> <span class="nb">str</span><span class="p">(</span><span class="n">CODE</span><span class="p">)</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s2">&quot;b&quot;</span><span class="p">)</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s2">&quot;&#39;&quot;</span><span class="p">)</span><span class="o">.</span><span class="n">strip</span><span class="p">(</span><span class="s1">&#39;&quot;&#39;</span><span class="p">)</span>

<span class="n">js</span> <span class="o">=</span> <span class="sa">f</span><span class="s2">&quot;&quot;&quot;function MyCopyCodeToForm()</span><span class="se">{{</span><span class="s2"></span>
<span class="s2">  var code = `</span><span class="si">{</span><span class="n">CODE</span><span class="si">}</span><span class="s2">`;</span>
<span class="s2">  document.getElementById(&#39;code&#39;).value = code;</span>
<span class="se">}}</span><span class="s2"></span>
<span class="s2">MyCopyCodeToForm();</span>
<span class="s2">var saveRunTest = document.getElementById(&#39;save&#39;);</span>
<span class="s2">saveRunTest.setAttribute(&#39;onclick&#39;, &#39;return MyCopyCodeToForm();&#39;)</span>
<span class="s2">saveRunTest.click();</span>
<span class="s2">&quot;&quot;&quot;</span>
<span class="n">driver</span><span class="o">.</span><span class="n">execute_script</span><span class="p">(</span><span class="n">js</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="s2">&quot;SAVE AND RUN TESTS&quot;</span><span class="p">)</span>
</pre></div>

    </div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area">

<div class="output_subarea output_stream output_stdout output_text">
<pre>SAVE AND RUN TESTS
</pre>
</div>
</div>

</div>
</div>

</div>
    {% endraw %}

</div>
 
