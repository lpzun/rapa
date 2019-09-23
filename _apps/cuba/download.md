---
title: Getting CUBA
permalink: /apps/cuba-download/
---
<html>
  <body>
      <div id="tool">
      <h3>1. Download</h3>
      The artifact is up on <a href="https://github.com/lpzun/cuba/tree/pldi">Github</a>. You can use the following command to check it out:
      <pre>
git clone -b pldi https://github.com/lpzun/cuba.git</pre>
      <b>Note:</b> The above command clones the <code>pldi</code> branch.
	<h3>2. Installation<br></h3>
	<h4>We have</h4>
	<ul>
	  <li>source code on <a href='https://github.com/lpzun/cuba/tree/pldi'>GitHub</a> or in the <code>src</code> directory if you checked out the whole artifact (for installation, see below).</li><br>
	</ul>
	<b>Note:</b> CUBA is being developed in C++11. Make sure that your compiler supports C++11 or above. CUBA has been successfully compiled with <code>g++</code>, <code>clang</code> and <code>cygwin</code>. 
	<h4>2.1 Install from source</h4>
	The following assumes you have checked out the source code from <a href='https://github.com/lpzun/cuba/tree/pldi'>GitHub</a>.
	<pre>
make
cd bin
./cuba -h
</pre>
	You can use <code>make clean</code> to remove the object files. For a
	thorough cleanup, which also removes the executable, use <code>make
	distclean</code>.

	The <code>Tutorial</code> tab contains more details about running the tool.
      </div>

      <h3>3. Programmer's Guide</h3>
      <div id="documentation">
	See <a href="../cuba-doc/html/index.html">here</a> for the off-the-shelf programmer's guide in <b>html</b> format.
	<h4>Generate programmer's guide</h4>
	<p>
	  The documentation is automatically generated using doxygen. To
	  (re-)generate the documentation, you must
	  have <a href="http://www.doxygen.org/">doxygen</a> installed on
	  your machine. Assuming you have checked out the Artifact submission
	  and entered the top-level directory,
	  <ol start="1" type="a">
	    <li><pre>cd doc</pre></li>
	    <li><pre>make</pre></li>
	  </ol>
	  To remove the documentation, use <pre>make clean</pre>
	</p>
      </div>
  </body>
</html>
