# LLM Finetuning



# Quantisation

**Conversion from higher memory format to a lower memory format.**
Ex: FP 32 - Full Precision each parameter in 32 bits

For larger models like Llama-2 70b; it has 70 billion parameters.
Too costly/not economical to store somewhere, will resolve this converting fp32 to int8.
Now, *Inference* is quicker and economical.
Makes fine-tuning easier, though it introduces loss.

## How to perform Quantization

### **Symmentric unit8 Quantisation** - Similar to Batch Normalisation.
```mermaid
graph LR
fp32[input - fp32] -- Quantisation --> uint8[output - uint8]
```
Ex : 
```mermaid
graph LR
a[0.0]-- before Quantisation -->b[1000]
a1[0]-- after Quantisation -->b1[255]
```

To scale it we use min-max scalar
$$
scale = {x_{max} - x_{min}\over q_{max} - q_{min}}
$$
and round it off
### **Assymmentric Quantisation** 

If data is skewed
```mermaid
graph LR
a2[-20.0]---->b2[1000]
```
To scale it we use min-max scalar
$$
scale = {x_{max} - x_{min}\over q_{max} - q_{min}}+zero\_point
$$
and round it off

## Post Training Quantisation (PTQ) : 

```mermaid
graph LR
a3[Pretrained Model]-- Traditional W&B -->b3[Calibration]-- Quantised W&B -->c3[Quantised Model]
```
There is a loss, leads to reduced accuarcy.
## Quantisation Aware Training (QAT) :

```mermaid
graph LR
a3[Pretrained Model]-- Traditional W&B -->b3[Quantisation]-- Quantised W&B -->c3[Finetuning]
d3[New Training Data]---->c3
```
As training data is later provided not depending on pretrained w&b accuracy is well maintained.

# LoRA and QLORA
```mermaid
graph LR
a3((GPT 4 Turbo, GPT 3.5 etc.))-- Full parameter fine-tuning -->b3((ChatGPT))-- Domain Specific Fine-tuning -->c3((Finance GPT etc))
b3-- Task Specific Fine-tuning --> d3((To do specific Task like Humaniser GPT))
```
## Full Parameter Fine-tuning

### Need to update all the weights(parameters) : 
### Hardware Resource Constraint : 
Downstream Task - Model Monitoring, Model inferencing

## LoRA
Instead of updating all the parameters, it tracks changes and adjust to extract fine-tuned weights.
LoRA proposes idea to decompose tracked weights matrix into 2 matrices(1xn and nx1).
$$
w_0 + \triangle w = w_0 +BA
$$
Lower the rank of decomposed matrices it is, lower the trainable parameters.
rank 1,2,8 are optimal ones.
higher ranks helps to learn complex things.


## QLORA - Quantised LoRA
weights are calibrated from fp16 to int4

## Export a file

You can export the current file by clicking **Export to disk** in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.


# Synchronization

Synchronization is one of the biggest features of StackEdit. It enables you to synchronize any file in your workspace with other files stored in your **Google Drive**, your **Dropbox** and your **GitHub** accounts. This allows you to keep writing on other devices, collaborate with people you share the file with, integrate easily into your workflow... The synchronization mechanism takes place every minute in the background, downloading, merging, and uploading file modifications.

There are two types of synchronization and they can complement each other:

- The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.
	> To start syncing your workspace, just sign in with Google in the menu.

- The file synchronization will keep one file of the workspace synced with one or multiple files in **Google Drive**, **Dropbox** or **GitHub**.
	> Before starting to sync files, you must link an account in the **Synchronize** sub-menu.

## Open a file

You can open a file from **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Open from**. Once opened in the workspace, any modification in the file will be automatically synced.

## Save a file

You can save any file of the workspace to **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Save on**. Even if a file in the workspace is already synced, you can save it to another location. StackEdit can sync one file with multiple locations and accounts.

## Synchronize a file

Once your file is linked to a synchronized location, StackEdit will periodically synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be resolved.

If you just have modified your file and you want to force syncing, click the **Synchronize now** button in the navigation bar.

> **Note:** The **Synchronize now** button is disabled if you have no file to synchronize.

## Manage file synchronization

Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking **File synchronization** in the **Synchronize** sub-menu. This allows you to list and remove synchronized locations that are linked to your file.


# Publication

Publishing in StackEdit makes it simple for you to publish online your files. Once you're happy with a file, you can publish it to different hosting platforms like **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **WordPress** and **Zendesk**. With [Handlebars templates](http://handlebarsjs.com/), you have full control over what you export.

> Before starting to publish, you must link an account in the **Publish** sub-menu.

## Publish a File

You can publish your file by opening the **Publish** sub-menu and by clicking **Publish to**. For some locations, you can choose between the following formats:

- Markdown: publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).

## Update a publication

After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the **Publish now** button in the navigation bar.

> **Note:** The **Publish now** button is disabled if your file has not been published yet.

## Manage file publication

Since one file can be published to multiple locations, you can list and manage publish locations by clicking **File publication** in the **Publish** sub-menu. This allows you to list and remove publication locations that are linked to your file.


# Markdown extensions

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
