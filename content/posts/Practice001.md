---
title: "Vim Practice001"
date: 2025-06-25
draft: false
---
# Macro

- q{character} to start recording a macro in register {character}
- q to stop recording
- @{character} replays the macro
- Macro execution stops on error
- {number}@{character} executes a macro {number} times
- Macros can be recursive
	first clear the macro with q{character}q
	record the macro, with @{character} to invoke the macro recursively (will be a no-op until recording is complete)

# Practice 1 Convert xml to json

## Step0
curl -O https://missing.csail.mit.edu/2020/files/example-data.xml

```xml
<person>

	<name>Johnny Zhang Jr.</name>

	<email>amyalvarez@cole.com</email>

</person>
```
![step0](portfolio/practice1_step0.gif)
## Step1: Macro to format a single element (register `e`)

`go to line with <name>`
<\name>Johnny Zhang Jr.</name\>

**qe** `start recording register macro e

**^** `go to the first character of this line which is <`
==<==name>Johnny Zhang Jr.</name\>

**r"** `replace < with "`
=="==name>Johnny Zhang Jr.</name\>

**f>** `find next >
"name ==\>== Johnny Zhang Jr.</name\>

**s": "** `delete character > and substitute with ": "`
"name==":  "==Johnny Zhang Jr.</name\>

f< `find next <`
"name":  "Johnny Zhang Jr.==\<==/name\>

C" `change to the end of the line`
"name":  "Johnny Zhang Jr.=="==

**q**  ` end the recording`


![step1](/portfolio/practice1_step1.gif)

## Step2: Macro to format a person (register p)

`go to line with <person>`

```
<person>

<name>Johnny Zhang Jr.</name>

<email>amyalvarez@cole.com</email>

</person>
```

**qp** `start recording register macro p

**S{**  `delete line and substitute with {`
==<==person> -> {

**\<ESC>j** `exit to normal mode and go to next line`
\<name>Johnny Zhang Jr.\<name>

**@e** `execute macro e 
"name":  "Johnny Zhang Jr.=="==

**A,** `append , to the end of the line`
"name":  "Johnny Zhang Jr."==,==

**j@e** `go to next line and execute macro e
"email": "amyalvarez@cole.com"

**jS}** ` go to the next line and delete the line </person> and su situte with },`
\<person> -> },

**\<ESC>q**`exit to normal mode and end the recording`


![step2](/portfolio/practice1_step2.gif)
## Step3: Macro to format a person and go to next person (register q)

**qq** `start recording register macro q

**@p** `execute macro p 

**j** `go to next person`

**q** `end the recording`

![step3](/portfolio/practice1_step3.gif)
## Step4: Execute macros until end of the file

**999@q**

![step4](/portfolio/practice1_step4.gif)



