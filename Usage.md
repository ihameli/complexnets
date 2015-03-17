# Graphical mode #

To start the application in graphical mode, just invoke `complexnetspp` whithout arguments from command line.
```
$ ./complexnetspp
```


# Command line mode #

ComplexNets++ can also run using the command line.

## Loading a graph from an input file ##

The syntax to load a graph from an input file is the following:
```
$ ./complexnetspp -i <filename>
```

If the file represents a weighted graph, the command must include the `--weighted` argument:
```
$ ./complexnetspp -i <filename> --weighted
```

If the file represents a digraph, the command must include the `--digraph` argument:
```
$ ./complexnetspp -i <filename> --digraph
```

NOTE: Currently, both `--weighted` AND `--digraph` arguments CANNOT be used at the same time, as weighted digraphs aren't still supported by complexnets.

## Creating a graph using a model ##

The graphs can also be created using a model. To achieve this, the command must have the following syntax:
```
$ ./complexnetspp <model_parameters>
```
Where `<model_parameters>` can take one of the following values depending on the used model:
  * Erdos-Renyi:
```
--erdos -n <number_of_nodes> -p <probability>
```
  * Barabási-Albert:
```
--barabasi -n <number_of_nodes> -c <number_of_initial_nodes> -m <number_of_attachment_nodes>
```
  * Extended Hot:
```
--hot -n <number_of_nodes> -m <new_vertex_edges> -x <neighbors_select> -q <added_edges> -r <edges_select> -t <root_regeneration_rate>
```
  * Molloy-Reed:
```
--molloy -k <ks_file>
```
  * Hyperbolic:
```
--hyperbolic --n=<number of nodes> -a=<radial density> --deg=<degree coefficient>
```

## Computing a property on a graph ##

To compute a property given a file, the command must have the following syntax:
```
$ ./complexnetspp <graph> <property>
```
where `<graph>` are the arguments to load a graph or create it using a model, and `<property>` can take the following values depending on the calculated property:
  * Betweenness:
```
--betweenness <vertex_index>
```
  * Degree distribution:
```
--ddist <degree>
```

NOTE: when working with a digraph, this command will show the number of vertices having `<degree>` as in-degree and out-degree at the same time.

  * Clustering coefficient:
```
--clustering <vertex_index>
```
  * Nearest neighbors degree:
```
--knn <vertex_index>
```
  * Shell index:
```
--shell <vertex_index>
```
  * Maximal clique:
```
 --maxCliqueExact <max_time>
 --maxCliqueAprox
```

> NOTE: The method maxCliqueExact takes to long. That's why it has a time-out (in seconds). If looking for a quick answer use the approximation method.<br />
> NOTE2: If you want to obtain a result the most accurate possible but with a time limit use:

```
 $ ./complexnetspp <graph> --maxCliqueExact <max_time> || ./complexnetspp <graph> --maxCliqueAprox
```

## Fitting data against a power law ##

To test a graph (either generated or loaded from a file) for a power law distribution, use the --print-deg option and redirect the output to the appropiate script. For instance:
```
$ ./complexnetspp <graph> --print-deg=1 | python powerlawpy/script.py
```

The script is independent to the project and requires several extra dependencies, mainly: python, python-dev, numpy, scigy. Please refer to the appropiate README for installation details.

## Saving a graph in a file ##

To save a loaded or model-generated graph in a file, the command must have the following syntax:
```
$ ./complexnetspp <graph> -o <file>
```
where `<graph>` are the arguments to load a graph or create it using a model.

## Generating a Gnuplot readable output ##

To generate a file that can be read and plotted by Gnuplot, the command must have the following syntax:
```
$ ./complexnetspp <graph> -o <output_file> <function>
```
where `<graph>` are the arguments to load a graph or create it using a model and `<function>` can take the following values depending on the calculated function:
  * Betweenness vs. Degree:
```
--betweenness-output
```
  * Degree distribution:
```
--ddist-output
```
> If the output is expected to be grouped using log-bins, the command must include the `log-bin` option:
```
--ddist-output --log-bin=<number_of_bins>
```

NOTE: when working with digraphs, this command will output the in-degree probabilities into a file with `_in_degree` suffix and the out-degree probabilities into a file with `_out_degree` suffix.

  * Clustering coefficient vs. Degree:
```
--clustering-output
```
  * Nearest neighbors degree vs. Degree:
```
--knn-output
```
  * Shell index vs. Degree:
```
--shell-output
```
  * Maximal clique distribution (for each node ¿witch is the biggest clique it is part of?):
```
--maxCliqueExact-output <max_time>
--maxCliqueAprox-output
```
## Using a bash script to generate plots ##

A bash script can be used to generate the plot for each property of a graph. To achieve this, the following command must be invoked:
```
$ sh plotter.sh <input_file>
```
This command creates a directory in which the plots for betweenness, degree distribution, clustering coefficient, nearest neighbors degree and shell index are saved for the graph represented by `<input_file>`. The input file must have an extension.

If the command is invoked with one more parameter:
```
$ sh plotter.sh <input_file> <number_of_bins>
```
then the degree distribution is also plotted using the log-bin method, with the number of bins specified by `<number_of_bins>`.