# visualise-spacy-tree

An alternative to [SpaCy's](https://spacy.io) built-in [visualizer](https://spacy.io/usage/visualizers#dep), built on [GraphViz](https://graphviz.gitlab.io/). 

![Custom plot image](https://github.com/cyclecycle/visualise-spacy-tree/blob/master/example_plots/custom_plot.png)

## Prequisites

- GraphViz. Download and install as per the instructions [here](https://graphviz.gitlab.io/download/)
- [pydot](https://github.com/pydot/pydot)

## Installation

With pip:

```bash
pip install visualise-spacy-tree
```

## Usage

```python

# First, parse a string to create SpaCy Doc object
import en_core_web_sm

text = 'Forging involves the shaping of metal using localized compressive forces.'

nlp = en_core_web_sm.load()
doc = nlp(text)

# Create the plot
import visualise_spacy_tree
plot = visualise_spacy_tree.plot(doc)  # It's .png format

# Write it to a file
with open('parse_tree.png', 'wb') as f:
    f.write(plot)

# Or if you're using Jupyter notebook, you can render it inline
from IPython.display import Image, display
display(Image(plot))

# Override node attributes using the underscore extension
from spacy.tokens import Token
Token.set_extension('plot', default={})
for token in doc:
    node_text = '{0} [{1}]\n({2} / {3})'.format(
            token.orth_,
            token.i,
            token.pos_,
            token.tag_
        )
    token._.plot['text'] = node_text

doc[0]._.plot['color'] = 'green'  # Make first token green
doc[1]._.plot['shape'] = 'box'  # Make second token box-shape

'''
See GraphViz docs for reference of possible node attributes:
https://graphviz.gitlab.io/_pages/doc/info/attrs.html
'''

```
