---
title: Experiments
permalink: /apps/quba-experiment/
---
<html>
<body>
    <div class="content">
        <div id="tool">
            <h3>1. Experimental Setup</h3>
            <p>
                For reference, our experiments are performed on a 2.80 GHz Intel(R) Core(TM) i7-7600 machine with 8 GB memory,
                running 64-bit Windows 10. The timeout per benchmark is set to 3600sec (1h); the memory limit
                to 4 GB.
            </p>
            <h3>2. Reproduce Experiments</h3>
            <p>
                The benchmarks used in our paper can be found in the directory<code>examples/cav19_bm</code>.
                Various batch scripts have been included for reproducing the experiments.
            </p>
            <p>
                <b><i>Remark</i>:</b>
                <ul>
                    <li>
                        For benchmark <code>01/German-1</code> and <code>02/German-2</code>, our script only reproduces
                        the experiments with invariants.
                    </li>
                    <br />
                    <li>
                        Per suggestion from the instruction of artifact submission, we exclude
                        from the artifact two time-consuming benchmarks <code>04/TokenRing-fixed</code> and <code>07/openWSN</code>.
                    </li>
                    <br />

                    <li>
                        We did not collect memory usage as doing this in Windows batch script is painful. We manually collected
                        the information of memory usage reported in the paper.
                    </li>
                </ul>
            </p>
            <h4>Step 1. Compile benchmarks</h4>
            To compile benchmarks, please execute the following command:
            <pre>compile-benchmark.bat</pre>

            To clean up benchmarks, please execute the following command:
            <pre>cleanup-benchmark.bat</pre>

            <h4>Step 2. Run experiments</h4>
            To reproduce the experiments, please execute the following command:
            <pre>run-experiments.bat</pre>

            The experiments will generate logs. Read the <code>README.txt</code> to get more on how to read the log files.

            <h4>Step 3. Process results</h4>

            <b><i>Warning</i>:</b> To use the process-results scripts, please make sure you have Python (version 2.7.14 or above) installed on your machine.
            <p>
                To process the experimental results, e.g. executing time etc., please execute the following command:
                <pre>process-results.bat</pre>

                After running the above script, the results will be stored in the following two files:
                <ul>
                    <li>
                        <b>pat_ptester.csv:</b> the results of running PAT and PTester atop which our tool is built; the comparison demonstrates the overhead our approach adds it on.
                    </li>
                    <li>
                        <b>pati.csv:</b> the experimental result of PAT+I; only <code>01/German-1</code> and <code>02/German-2</code> have such results.
                    </li>
                </ul>
            </p>
        </div>

    </div>
</body>
</html>
