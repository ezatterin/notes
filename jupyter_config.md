# Jupyter configs

Generate the config files for a given Jupyter app as (e.g. for the notebook)

```bash
$ jupyter notebook --generate-config
```

This will be placed in `~/.jupyter/jupyter_notebook_config.py'. Inside there are commands which can also be passed as arguments 
when calling the app, e.g. `c.NotebookApp.port = 8888` which corresponds to

```bash
$ jupyter notebook --port=8888
```

Other configs are contained in what used to be the profile directory in `.ipython`. It does not seem that Jupyter can handle profiles. These are created and managed by IPython, 

```bash
$ ipython profile list
```

to see which are there. The default profile is the one used by Jupyter when calling its apps. Perhaps it can be changed in the config files... I don't know. Anywas, to take all the config file of the IPython default profile and migrate them to Jupyter

```bash 
$ jupyter migrate
```

which moves the `custom.css` and `custom.json` files to `~/.jupyter/custom` *provided they are not empty*. I guess these are the ones that should be modified then.

**TODO**: how to handle profiles? How to run a server?
