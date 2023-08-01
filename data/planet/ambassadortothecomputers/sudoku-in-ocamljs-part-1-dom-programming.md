---
title: 'Sudoku in ocamljs, part 1: DOM programming'
description: Let's make a Sudoku game with ocamljs  and the Dom  library for programming
  the browser DOM. Like on the cooking shows, I have prepared the ...
url: http://ambassadortothecomputers.blogspot.com/2009/04/sudoku-in-ocamljs-part-1-dom.html
date: 2009-04-27T05:30:00-00:00
preview_image:
featured:
authors:
- Jake Donham
source:
---

<p>Let's make a Sudoku game with <a href="http://code.google.com/p/ocamljs">ocamljs</a> and the <a href="http://code.google.com/p/ocamljs/wiki/Dom"><code>Dom</code></a> library for programming the browser DOM. Like on the cooking shows, I have prepared the dish we're about to make beforehand; why don't you <a href="http://ocamljs.googlecode.com/svn/examples/dom/sudoku/index.html">taste it now</a>? OK, it is not yet Sudoku, lacking the important ingredient of some starting numbers to guide the game--we'll come back to that next time. </p><pre><span class="htmlize-tuareg-font-lock-governing">module</span> <span class="htmlize-type">D </span><span class="htmlize-tuareg-font-lock-operator">=</span> Dom
<span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">d </span><span class="htmlize-tuareg-font-lock-operator">=</span> <span class="htmlize-type">D</span>.document
</pre>We begin with some definitions. The <code>Dom</code> module includes class types for much of the standard browser DOM, using the ocamljs facility for <a href="http://code.google.com/p/ocamljs/wiki/Interfacing">interfacing with Javascript objects</a>. <code>Dom.document</code> is the browser document object. <pre><span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">make_board</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">()</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span>
  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">make_input</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">()</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">input </span><span class="htmlize-tuareg-font-lock-operator">=</span> <span class="htmlize-tuareg-font-lock-operator">(</span>d<span class="htmlize-tuareg-font-lock-operator">#</span>createElement <span class="htmlize-string">&quot;input&quot;</span> <span class="htmlize-tuareg-font-lock-operator">:</span> <span class="htmlize-type">D.input</span><span class="htmlize-tuareg-font-lock-operator">)</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    input<span class="htmlize-tuareg-font-lock-operator">#</span>setAttribute <span class="htmlize-string">&quot;type&quot;</span> <span class="htmlize-string">&quot;text&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
    input<span class="htmlize-tuareg-font-lock-operator">#</span>_set_size 1<span class="htmlize-tuareg-font-lock-operator">;</span>
    input<span class="htmlize-tuareg-font-lock-operator">#</span>_set_maxLength 1<span class="htmlize-tuareg-font-lock-operator">;</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">style </span><span class="htmlize-tuareg-font-lock-operator">=</span> input<span class="htmlize-tuareg-font-lock-operator">#</span>_get_style <span class="htmlize-tuareg-font-lock-governing">in</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_border <span class="htmlize-string">&quot;none&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_padding <span class="htmlize-string">&quot;0px&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">enforce_digit</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">()</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span>
      <span class="htmlize-keyword">match</span> input<span class="htmlize-tuareg-font-lock-operator">#</span>_get_value <span class="htmlize-keyword">with</span>
        <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;1&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;2&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;3&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;4&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;5&quot;</span>
        <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;6&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;7&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;8&quot;</span> <span class="htmlize-tuareg-font-lock-operator">|</span> <span class="htmlize-string">&quot;9&quot;</span> <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> <span class="htmlize-tuareg-font-lock-operator">()</span>
        <span class="htmlize-tuareg-font-lock-operator">|</span> _ <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> input<span class="htmlize-tuareg-font-lock-operator">#</span>_set_value <span class="htmlize-string">&quot;&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    input<span class="htmlize-tuareg-font-lock-operator">#</span>_set_onchange <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-type">Ocamljs</span>.jsfun enforce_digit<span class="htmlize-tuareg-font-lock-operator">);</span>
    input <span class="htmlize-tuareg-font-lock-governing">in</span>
</pre>We construct the Sudoku board in several steps. First, we make an input box for each square. Notice that you can call DOM methods (e.g. <code>createElement</code>) with OCaml object syntax. But what is the type of <code>createElement</code>? The type of the object you get back depends on the tag name you pass in; OCaml has no type for that. So <code>createElement</code> is declared to return <code>#element</code> (that is, a subclass of <code>element</code>). If you need only methods from <code>element</code> then you usually don't need to ascribe a more-specific type, but in this case we need an <code>input</code> node. (Static type checking with Javascript objects is therefore only advisory in some cases--if you ascribe the wrong type you can get a runtime error--but still better than nothing.)<br/>
<p>We next set some attributes, properties, and styles on the input box. Properties are manipulated with specially-named methods: <code>foo#_get_bar</code> becomes <code>foo.bar</code> in Javascript, and <code>foo#_set_bar baz</code> becomes <code>foo.bar = baz</code>. Finally we add a validation function to enforce that the input box contains at most a single digit. To set the <code>onchange</code> handler, you need to wrap it in <code>Ocamljs.jsfun</code>, because the calling convention of an ocamljs function is different from that of plain Javascript function (to accomodate partial application and tail recursion).<br/>
</p><pre>  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">make_td</span><span class="htmlize-variable-name"> i j input </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">td </span><span class="htmlize-tuareg-font-lock-operator">=</span> d<span class="htmlize-tuareg-font-lock-operator">#</span>createElement <span class="htmlize-string">&quot;td&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">style </span><span class="htmlize-tuareg-font-lock-operator">=</span> td<span class="htmlize-tuareg-font-lock-operator">#</span>_get_style <span class="htmlize-tuareg-font-lock-governing">in</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_borderStyle <span class="htmlize-string">&quot;solid&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_borderColor <span class="htmlize-string">&quot;#000000&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">widths</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span> <span class="htmlize-keyword">function</span>
     <span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">|</span> 0 <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 2<span class="htmlize-tuareg-font-lock-operator">,</span> 0 <span class="htmlize-tuareg-font-lock-operator">|</span> 2 <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 1<span class="htmlize-tuareg-font-lock-operator">,</span> 1 <span class="htmlize-tuareg-font-lock-operator">|</span> 3 <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 1<span class="htmlize-tuareg-font-lock-operator">,</span> 0
      <span class="htmlize-tuareg-font-lock-operator">|</span> 5 <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 1<span class="htmlize-tuareg-font-lock-operator">,</span> 1 <span class="htmlize-tuareg-font-lock-operator">|</span> 6 <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 1<span class="htmlize-tuareg-font-lock-operator">,</span> 0 <span class="htmlize-tuareg-font-lock-operator">|</span> 8 <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 1<span class="htmlize-tuareg-font-lock-operator">,</span> 2
      <span class="htmlize-tuareg-font-lock-operator">|</span> _ <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> 1<span class="htmlize-tuareg-font-lock-operator">,</span> 0 <span class="htmlize-tuareg-font-lock-governing">in</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-variable-name">top</span><span class="htmlize-tuareg-font-lock-operator">,</span><span class="htmlize-variable-name"> bottom</span><span class="htmlize-tuareg-font-lock-operator">)</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span> widths i <span class="htmlize-tuareg-font-lock-governing">in</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-variable-name">left</span><span class="htmlize-tuareg-font-lock-operator">,</span><span class="htmlize-variable-name"> right</span><span class="htmlize-tuareg-font-lock-operator">)</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span> widths j <span class="htmlize-tuareg-font-lock-governing">in</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">px</span><span class="htmlize-variable-name"> k </span><span class="htmlize-tuareg-font-lock-operator">=</span> string_of_int k <span class="htmlize-tuareg-font-lock-operator">^</span> <span class="htmlize-string">&quot;px&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_borderTopWidth <span class="htmlize-tuareg-font-lock-operator">(</span>px top<span class="htmlize-tuareg-font-lock-operator">);</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_borderBottomWidth <span class="htmlize-tuareg-font-lock-operator">(</span>px bottom<span class="htmlize-tuareg-font-lock-operator">);</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_borderLeftWidth <span class="htmlize-tuareg-font-lock-operator">(</span>px left<span class="htmlize-tuareg-font-lock-operator">);</span>
    style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_borderRightWidth <span class="htmlize-tuareg-font-lock-operator">(</span>px right<span class="htmlize-tuareg-font-lock-operator">);</span>
    ignore <span class="htmlize-tuareg-font-lock-operator">(</span>td<span class="htmlize-tuareg-font-lock-operator">#</span>appendChild input<span class="htmlize-tuareg-font-lock-operator">);</span>
    td <span class="htmlize-tuareg-font-lock-governing">in</span>
</pre>Next we make a table cell for each square, containing the input box, with borders according to its position in the grid. Here we don't ascribe a type to the result of <code>createElement</code> since we don't need any <code>td</code>-specific methods.<br/>
<pre>  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">rows </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    <span class="htmlize-type">Array</span>.init 9 <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">i </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
      <span class="htmlize-type">Array</span>.init 9 <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">j </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
        make_input <span class="htmlize-tuareg-font-lock-operator">()))</span> <span class="htmlize-tuareg-font-lock-governing">in</span>

  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">table </span><span class="htmlize-tuareg-font-lock-operator">=</span> d<span class="htmlize-tuareg-font-lock-operator">#</span>createElement <span class="htmlize-string">&quot;table&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
  table<span class="htmlize-tuareg-font-lock-operator">#</span>setAttribute <span class="htmlize-string">&quot;cellpadding&quot;</span> <span class="htmlize-string">&quot;0px&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
  table<span class="htmlize-tuareg-font-lock-operator">#</span>setAttribute <span class="htmlize-string">&quot;cellspacing&quot;</span> <span class="htmlize-string">&quot;0px&quot;</span><span class="htmlize-tuareg-font-lock-operator">;</span>
  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">tbody </span><span class="htmlize-tuareg-font-lock-operator">=</span> d<span class="htmlize-tuareg-font-lock-operator">#</span>createElement <span class="htmlize-string">&quot;tbody&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
  ignore <span class="htmlize-tuareg-font-lock-operator">(</span>table<span class="htmlize-tuareg-font-lock-operator">#</span>appendChild tbody<span class="htmlize-tuareg-font-lock-operator">);</span>
  <span class="htmlize-type">ArrayLabels</span>.iteri rows <span class="htmlize-tuareg-font-lock-operator">~</span><span class="htmlize-variable-name">f</span><span class="htmlize-tuareg-font-lock-operator">:(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">i row </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">tr </span><span class="htmlize-tuareg-font-lock-operator">=</span> d<span class="htmlize-tuareg-font-lock-operator">#</span>createElement <span class="htmlize-string">&quot;tr&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    <span class="htmlize-type">ArrayLabels</span>.iteri row <span class="htmlize-tuareg-font-lock-operator">~</span><span class="htmlize-variable-name">f</span><span class="htmlize-tuareg-font-lock-operator">:(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">j cell </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
      <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">td </span><span class="htmlize-tuareg-font-lock-operator">=</span> make_td i j cell <span class="htmlize-tuareg-font-lock-governing">in</span>
      ignore <span class="htmlize-tuareg-font-lock-operator">(</span>tr<span class="htmlize-tuareg-font-lock-operator">#</span>appendChild td<span class="htmlize-tuareg-font-lock-operator">));</span>
    ignore <span class="htmlize-tuareg-font-lock-operator">(</span>tbody<span class="htmlize-tuareg-font-lock-operator">#</span>appendChild tr<span class="htmlize-tuareg-font-lock-operator">));</span>

  <span class="htmlize-tuareg-font-lock-operator">(</span>rows<span class="htmlize-tuareg-font-lock-operator">,</span> table<span class="htmlize-tuareg-font-lock-operator">)</span>
</pre>Then we assemble the full board: make a 9 x 9 matrix of input boxes, make a table containing the input boxes, then return the matrix and table. Notice that we freely use the OCaml standard library. Here the <code>tbody</code> is necessary for IE; the <code>cellpadding</code> and <code>cellspacing</code> don't work in IE for some reason that I have not tracked down. This raises an important point: the <code>Dom</code> module is the thinnest possible wrapper over the actual DOM objects, and as such gives you no help with cross-browser compatibility. <pre><span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">check_board</span><span class="htmlize-variable-name"> rows _ </span><span class="htmlize-tuareg-font-lock-operator">=</span>
  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">error</span><span class="htmlize-variable-name"> i j </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">cell </span><span class="htmlize-tuareg-font-lock-operator">=</span> rows.<span class="htmlize-tuareg-font-lock-operator">(</span>i<span class="htmlize-tuareg-font-lock-operator">)</span>.<span class="htmlize-tuareg-font-lock-operator">(</span>j<span class="htmlize-tuareg-font-lock-operator">)</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    cell<span class="htmlize-tuareg-font-lock-operator">#</span>_get_style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_backgroundColor <span class="htmlize-string">&quot;#ff0000&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>

  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">check_set</span><span class="htmlize-variable-name"> set </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">seen </span><span class="htmlize-tuareg-font-lock-operator">=</span> <span class="htmlize-type">Array</span>.make 9 None <span class="htmlize-tuareg-font-lock-governing">in</span>
    <span class="htmlize-type">ArrayLabels</span>.iter set <span class="htmlize-tuareg-font-lock-operator">~</span><span class="htmlize-variable-name">f</span><span class="htmlize-tuareg-font-lock-operator">:(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-variable-name">i</span><span class="htmlize-tuareg-font-lock-operator">,</span><span class="htmlize-variable-name">j</span><span class="htmlize-tuareg-font-lock-operator">)</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
      <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">cell </span><span class="htmlize-tuareg-font-lock-operator">=</span> rows.<span class="htmlize-tuareg-font-lock-operator">(</span>i<span class="htmlize-tuareg-font-lock-operator">)</span>.<span class="htmlize-tuareg-font-lock-operator">(</span>j<span class="htmlize-tuareg-font-lock-operator">)</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
      <span class="htmlize-keyword">match</span> cell<span class="htmlize-tuareg-font-lock-operator">#</span>_get_value <span class="htmlize-keyword">with</span>
        <span class="htmlize-tuareg-font-lock-operator">|</span> &quot;&quot; <span class="htmlize-tuareg-font-lock-operator">-&gt;</span> <span class="htmlize-tuareg-font-lock-operator">()</span><span class="htmlize-string">
 </span>       <span class="htmlize-tuareg-font-lock-operator">|</span> v <span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
            <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">n </span><span class="htmlize-tuareg-font-lock-operator">=</span> int_of_string v <span class="htmlize-tuareg-font-lock-governing">in</span>
            <span class="htmlize-keyword">match</span> seen.<span class="htmlize-tuareg-font-lock-operator">(</span>n <span class="htmlize-tuareg-font-lock-operator">-</span> 1<span class="htmlize-tuareg-font-lock-operator">)</span> <span class="htmlize-keyword">with</span>
              <span class="htmlize-tuareg-font-lock-operator">|</span> None <span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
                  seen.<span class="htmlize-tuareg-font-lock-operator">(</span>n <span class="htmlize-tuareg-font-lock-operator">-</span> 1<span class="htmlize-tuareg-font-lock-operator">)</span> <span class="htmlize-tuareg-font-lock-operator">&lt;-</span> Some <span class="htmlize-tuareg-font-lock-operator">(</span>i<span class="htmlize-tuareg-font-lock-operator">,</span>j<span class="htmlize-tuareg-font-lock-operator">)</span>
              <span class="htmlize-tuareg-font-lock-operator">|</span> Some <span class="htmlize-tuareg-font-lock-operator">(</span>i<span class="htmlize-tuareg-font-lock-operator">',</span>j<span class="htmlize-tuareg-font-lock-operator">')</span> <span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
                  error i j<span class="htmlize-tuareg-font-lock-operator">;</span>
                  error i<span class="htmlize-tuareg-font-lock-operator">'</span> j<span class="htmlize-tuareg-font-lock-operator">')</span> <span class="htmlize-tuareg-font-lock-governing">in</span>

  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">check_row</span><span class="htmlize-variable-name"> i </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    check_set <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-type">Array</span>.init 9 <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">j </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span> <span class="htmlize-tuareg-font-lock-operator">(</span>i<span class="htmlize-tuareg-font-lock-operator">,</span>j<span class="htmlize-tuareg-font-lock-operator">)))</span> <span class="htmlize-tuareg-font-lock-governing">in</span>

  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">check_column</span><span class="htmlize-variable-name"> j </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    check_set <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-type">Array</span>.init 9 <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">i </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span> <span class="htmlize-tuareg-font-lock-operator">(</span>i<span class="htmlize-tuareg-font-lock-operator">,</span>j<span class="htmlize-tuareg-font-lock-operator">)))</span> <span class="htmlize-tuareg-font-lock-governing">in</span>

  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">check_square</span><span class="htmlize-variable-name"> i j </span><span class="htmlize-tuareg-font-lock-operator">=</span>
    <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">set </span><span class="htmlize-tuareg-font-lock-operator">=</span> <span class="htmlize-type">Array</span>.init 9 <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">k </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
      i <span class="htmlize-tuareg-font-lock-operator">*</span> 3 <span class="htmlize-tuareg-font-lock-operator">+</span> k <span class="htmlize-tuareg-font-lock-operator">mod</span> 3<span class="htmlize-tuareg-font-lock-operator">,</span> j <span class="htmlize-tuareg-font-lock-operator">*</span> 3 <span class="htmlize-tuareg-font-lock-operator">+</span> k <span class="htmlize-tuareg-font-lock-operator">/</span> 3<span class="htmlize-tuareg-font-lock-operator">)</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
    check_set set <span class="htmlize-tuareg-font-lock-governing">in</span>

  <span class="htmlize-type">ArrayLabels</span>.iter rows <span class="htmlize-tuareg-font-lock-operator">~</span><span class="htmlize-variable-name">f</span><span class="htmlize-tuareg-font-lock-operator">:(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">row </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
    <span class="htmlize-type">ArrayLabels</span>.iter row <span class="htmlize-tuareg-font-lock-operator">~</span><span class="htmlize-variable-name">f</span><span class="htmlize-tuareg-font-lock-operator">:(</span><span class="htmlize-keyword">fun</span> <span class="htmlize-variable-name">cell </span><span class="htmlize-tuareg-font-lock-operator">-&gt;</span>
      cell<span class="htmlize-tuareg-font-lock-operator">#</span>_get_style<span class="htmlize-tuareg-font-lock-operator">#</span>_set_backgroundColor <span class="htmlize-string">&quot;#ffffff&quot;</span><span class="htmlize-tuareg-font-lock-operator">));</span>

  <span class="htmlize-keyword">for</span> i <span class="htmlize-tuareg-font-lock-operator">=</span> 0 <span class="htmlize-keyword">to</span> 8 <span class="htmlize-keyword">do</span> check_row i <span class="htmlize-keyword">done</span><span class="htmlize-tuareg-font-lock-operator">;</span>
  <span class="htmlize-keyword">for</span> j <span class="htmlize-tuareg-font-lock-operator">=</span> 0 <span class="htmlize-keyword">to</span> 8 <span class="htmlize-keyword">do</span> check_column j <span class="htmlize-keyword">done</span><span class="htmlize-tuareg-font-lock-operator">;</span>
  <span class="htmlize-keyword">for</span> i <span class="htmlize-tuareg-font-lock-operator">=</span> 0 <span class="htmlize-keyword">to</span> 2 <span class="htmlize-keyword">do</span>
    <span class="htmlize-keyword">for</span> j <span class="htmlize-tuareg-font-lock-operator">=</span> 0 <span class="htmlize-keyword">to</span> 2 <span class="htmlize-keyword">do</span>
      check_square i j
    <span class="htmlize-keyword">done</span>
  <span class="htmlize-keyword">done</span><span class="htmlize-tuareg-font-lock-operator">;</span>
  <span class="htmlize-constant">false</span>
</pre>Now we define a function to check that the Sudoku constraints are satisfied: that no row, column, or heavy-lined square has more than one occurrence of a digit. If more than one digit occurs then we color all occurrences red. The only ocamljs-specific parts here are getting the cell contents (with <code>_get_value</code>) and setting the background color style. However, it's worth noticing the algorithm: we imperatively clear the error states for all cells, then set error states as we check each constraint. I'll revisit this in a later post about functional reactive programming. <pre><span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-function-name">onload</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">()</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span>
  <span class="htmlize-tuareg-font-lock-governing">let</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-variable-name">rows</span><span class="htmlize-tuareg-font-lock-operator">,</span><span class="htmlize-variable-name"> table</span><span class="htmlize-tuareg-font-lock-operator">)</span><span class="htmlize-variable-name"> </span><span class="htmlize-tuareg-font-lock-operator">=</span> make_board <span class="htmlize-tuareg-font-lock-operator">()</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">check </span><span class="htmlize-tuareg-font-lock-operator">=</span> d<span class="htmlize-tuareg-font-lock-operator">#</span>getElementById <span class="htmlize-string">&quot;check&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
  check<span class="htmlize-tuareg-font-lock-operator">#</span>_set_onclick <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-type">Ocamljs</span>.jsfun <span class="htmlize-tuareg-font-lock-operator">(</span>check_board rows<span class="htmlize-tuareg-font-lock-operator">));</span>
  <span class="htmlize-tuareg-font-lock-governing">let</span> <span class="htmlize-variable-name">board </span><span class="htmlize-tuareg-font-lock-operator">=</span> d<span class="htmlize-tuareg-font-lock-operator">#</span>getElementById <span class="htmlize-string">&quot;board&quot;</span> <span class="htmlize-tuareg-font-lock-governing">in</span>
  ignore <span class="htmlize-tuareg-font-lock-operator">(</span>board<span class="htmlize-tuareg-font-lock-operator">#</span>appendChild table<span class="htmlize-tuareg-font-lock-operator">)</span>

<span class="htmlize-tuareg-font-lock-operator">;;</span>

<span class="htmlize-type">D</span>.window<span class="htmlize-tuareg-font-lock-operator">#</span>_set_onload <span class="htmlize-tuareg-font-lock-operator">(</span><span class="htmlize-type">Ocamljs</span>.jsfun onload<span class="htmlize-tuareg-font-lock-operator">)</span>
</pre>Finally we put the pieces together: make the board, insert it into the DOM, call <code>check_board</code> when the Check button is clicked, and call this setup code once the document has been loaded. See the <a href="http://code.google.com/p/ocamljs/source/browse/#svn/trunk/examples/dom/sudoku">full source</a> for build files.<br/>
<p>By writing this in OCaml rather than directly in Javascript, we've gained the assurance of static type checking; we get to use OCaml's syntax, pattern matching, and standard library; we have a <a href="http://math.andrej.com/2009/04/09/pythons-lambda-is-broken/">for loop that's not broken</a>. On the flip side we have to worry about type ascription and <code>Ocamljs.jsfun</code>. If you don't already think that OCaml is a better language than Javascript, this won't convince you. But perhaps the followup posts, in which I'll show how to use RPC over HTTP with <a href="http://code.google.com/p/orpc2/">orpc</a> and functional reactive programming with <a href="http://code.google.com/p/froc/">froc</a>, will tip the scales for you.<br/>
</p>