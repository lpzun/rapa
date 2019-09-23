---
title: Tutorial
permalink: /apps/quba-tutorial/
---
<html>
<body>
	<h4>Prerequisites</h4>
            <p>
                <ul>
                    <li>
                        Our tool requires <b>64-bit Windows 10</b>. Using <b>Windows Command Prompt</b> is most effective.
                    </li>
                    <br />
                    <li>
                        To better understand our tool PAT, please read the design description under <code>Documentation</code> tab first.
                    </li>
                    <br />
                    <li>
                        For this tutorial, please enter the <code>examples/tutorial/</code> directory.
                    </li>
                    <br />
                    <li>
                        To have the executable of our tool readily available, please add the
                        location <code>your-unpacked-path /artifact/bin/x64/Binaries/</code> to the PATH
                        environment variable.
                    </li>
                </ul>
            </p>
            <h4>1. Help Info</h4>
            The following command prints usage help info:
            <pre><code>pt /h</code></pre>
            Some options related to our tool (PAT = <code>pt /os-list</code>) are explained as follows
            <pre>
---------------------------------------------
Options ::
---------------------------------------------
/h                       Print the help message

Flags related to exhaustive state space exploration:
/dfs                     Perform DFS exploration of the state space
/os-list                 Perform OS exploration (based on DFS) of the state space, with queue list abstraction
/queue-bound:k           Bound queue size to k (i.e. a machine's send is disabled when its current buffer is 
                         size k) (default: 0=unbounded for DFS, 1 for OS).
                         In case of /os-list search, this bound applies to the first-round queue and is 
                         incremented subsequently.
/queue-prefix:p          Keep prefix of queue of length p(>=0) /exact/ (abstraction applies to suffix starting 
                         at position p) (default: 0)
/interactive             Interactive mode: need users to press button to increase queue bound and continue 
                         exploration
/file-dump               Pretty-print accumulated states into files. For debugging only; this may create LARGE 
                         files!

Flags related to QuTL model checker:
/qutl:formula            Use QuTL formula to specify the properties which states should satisfy
/concrete                Invoke concrete model checker to test the correctness of QuTL formula!

If none of /psharp, /dfs, /os-... are specified: perform random testing</pre>
            </p>
            <p>
                We illustrate tool usage through a motivating example, <a href="../examples/tutorial/pingflood/pingflood.p">pingflood.p</a>,
                included in the <code>examples/tutorial/pingflood/</code> directory. It is the same example we show in Section 2 of the paper
                except that some functions/variables use different names.
            </p>
            <h4>2. Compile a P program</h4>
            <p>
                A P compiler <code>pc.exe</code> is included in the directory <code>bin/x64/Binaries</code>, and a batch
                script to compile <code>pingflood.p</code> is also included in <code>examples/tutorial/pingflood/</code>.
                So, simply run the following command to compile <code>pingflood.p</code>.
                <pre>build.bat</pre>
                The compilation produces a file named <code>pingflood.dll</code>, which is used by PAT, as shown below.
            </p>
            <b><i>Warning:</i></b> The compilation might take several minutes.
            <h4>3. Run PAT</h4>
            <b><i>Remark</i>:</b> verify P programs via iteratively increasing the unabstracted prefix of queues.
            For example,
            <pre>pt /os-list pingflood.dll /queue-prefix:0
pt /os-list pingflood.dll /queue-prefix:1
pt /os-list pingflood.dll /queue-prefix:2
pt /os-list pingflood.dll /queue-prefix:3
pt /os-list pingflood.dll /queue-prefix:4</pre>
            <b><i>Explanation</i>:</b>
            <ul>
                <li><code>pingflood.dll</code>: a compiled P program which is the target of verification.</li>
                <li><code>/os-list              </code>: use the <b>list abstraction</b>.</li>
                <li>
                    <code>/queue-prefix</code>: the size of the un-abstracted prefix of queues (default value is 0). During verification,
                    this value will
                    be iteratively increased, until you either experience <i>convergence</i>, or reach an (artificially specified) upper bound.
                </li>
            </ul>
            <b><i>Example</i>:</b> The example does NOT converge when the value of <code>/queue-prefix</code> is <code>0~3</code>.
            But it converges when the value of <code>queue-prefix</code> is <code>4</code>. The output of <code>queue-prefix:4</code>
            is shown at the bottom of this page. Whenever there is a plateau but meanwhile our tool cannot determine the convergence,
            our tool provides two options to users:
<pre>
(c)ontinue, by increasing the queue bound, ignoring the unreached successor, OR
(i)nvestigate; we will then locate the state with that hash code.</pre>

            The second one <code>(i)nvestigate</code> typically tell users why convergence does not appear at this point.
            Subsequently, users might be able to get some hints to design queue invariants to exclude spurious abstract states, if any.
            <h4>4. Run PAT+I</h4>
            <p>
                Other than iteratively increasing the un-abstracted prefix of an abstract queue, an alternative solution is to
                specify some invariant, via a QuTL formula, to exclude the spurious abstract states.
            </p>
            <p>
                <b><i>Remark</i>:</b> PAT+Invariant will enable QuTL model checker to exclude some spurious abstract states, which will prevent convergence. In the current
                implementation, the QuTL formula is written in
                <a href="https://en.wikipedia.org/wiki/Reverse_Polish_notation">Reverse Polish Notation (RPN)</a>
                (i.e. postfix notation). This is a limitation of the usability. We will improve this and use the
                infix notation in the next version.
            </p>
            <p>
                One thing we hope users keep in mind is that: Typically, there are many different helpful invariants.
                For instance, for the given example, one invariant is the same as we present in the paper.
                The other one is as follows:
                <pre><code>pt /os-list pingflood.dll /qutl:"true:$ DONE = DONE # 0 = => () PING PING G => () G &"</code></pre>
                We have included both invariants in the batch script <code>run.bat</code> under directory <code>tutorial/pingflood/</code>.
            </p>
            <b><i>Explanation</i>:</b>
            <ul>
                <li>
                    <code>/qutl:formula</code>:
                    The entire formula is a collection of machine-wise QuTL subformulae. The subformulae are separated by semicolon (;).
                    For example, program <code>pingflood.p</code> has two machines. So, the formula in the example
                    has two subformulae:
                    <br>
                    <ul>
                        <li>The QuTL subformula for machine <code>server</code> (the <code>Main</code> machine in the code) is: <code>true</code></li>
                        <br />
                        <li>
                            The QuTL subformula for machine <code>client</code> is:
                            <code>$ DONE = DONE # 0 = => () PING PING G => () G &</code>,
                            whose infix form is <code>($ = DONE => #DONE = 0) & G(PING => G PING)</code>.
                            <p>
                                <b><i>Remark</i>:</b> This subformula consists of two components, connected by logic and <code>&</code>.
                                They actually present a same constraint from different perspectives.
                                In other words, The first one is a supplementary of the second one.
                                Symbol <code>$</code> represents the current processing event. So, the first component says: <i>
                                    if the current
                                    processing event is <code>DONE</code>, then there won't be event <code>DONE</code> any more
                                </i>. While the second component says: <i>
                                    whenever an <code>PING</code> appears in the queue, then the incoming event is always <code>PING</code>
                                </i>.
                                Since the second component cannot constrain the processing event,
                                we need the first component to complete the constraint.
                            </p>
                        </li>
                        </ul>
                    </li>
            </ul>
            <b><i>Example</i>:</b> The example converges with the invariant, even when <code>queue-prefix:0</code>.
            The output is shown at the bottom of this page.
            <h4>5. Example Output</h4>
            <div class="row">
                <div class="column">
                    <p><i>PAT (with <code>/queue-prefix:p</code>)</i></p>
                    <object width="380" height="650" data="../img/pingflood-pat4.txt"></object>
                </div>
                <div class="column">
                    <p><i>PAT+I (with <code>/qutl:formula</code>)</i></p>
                    <object width="380" height="650" data="../img/pingflood-pati.txt"></object>
                </div>
            </div>
	</body>
</html>

