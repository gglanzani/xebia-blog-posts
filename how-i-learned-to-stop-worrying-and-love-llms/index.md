---
author: Giovanni Lanzani
brand: xebia.com
date: 2023-06-01 16:20:00+02:00
email: giovanni.lanzani@xebia.com
focus-keywords: llms chatgpt llm copilot go messagebird
guid: https://xebia.com/wp-json/wp/v2/posts/64979
image: images/banner.jpg
categories:
- data-science-and-ai
- python
- technology
og:
  image: images/og-banner.jpg
slug: how-i-learned-to-stop-worrying-and-love-llms
status: publish
subtitle: Writing your first Go app with ChatGPT
title: How I Learned to Stop Worrying and Love LLMs
---

<p style="font-size: 1.2em">In the sea of AI, there's a new threat on the horizon: Large Language Models (LLMs) and products built upon them, like ChatGPT and GitHub Copilot. Are they a tsunami about to swipe us away or a giant wave we must learn to ride?</p>

<p style="font-size: 1.1em; font-style: italic;">To investigate, I started with a concrete itch I had to scratch: sending a reminder to inform our customers, in bulk, by SMS. With ChatGPT, I created an application in a new programming language, Go, in minutes instead of days. I'll walk through how I did that and what it was like, and I'll close by reflecting on how, for programmers trapped doing the same things repeatedly, with minimal variations, LLMs might turn into a frightening tsunami. However, for the nimble kind, LLMs are an incredible wave to ride: you surf higher and faster if you know your moves, and that's a lot of fun.</p>

Since the launch of GitHub Copilot and ChatGPT, some people have been saying programmers will disappear as we know them today. In contrast, others say that programmers will be fine as long as the tools just spit out a statistically probable answer without understanding what you ask. 

Nowadays, I don't code much, making it hard to form an opinion based on first-hand experience. This changed last week. Some customers asked us to be reminded of an upcoming training, and we wanted to send an SMS instead of an email. As we didn't have any system in place for this, a colleague asked me for advice. I knew that messaging platforms provide APIs to send SMS, so I told him I would build a tool for the task. Selecting a platform was easy: I know MessageBird offers this service, is close to our Amsterdam office, and they are currently facing [headwinds]. That's the equivalent of a farmers market for an API, I thought, so I created an account.

MessageBird offers Python bindings to send SMSs with a simple API:

```python
client = messagebird.Client(ACCESS_KEY)
message = client.message_create(
  sender,
  receiver,
  message_text
)
```

Coding an app around it should be quick, as I'm still well-versed in Python.

But then it dawned on me: the next time a colleague needs to send SMS in bulk, I'll have to do it for them, as getting a working Python environment is hard if you're not technical. What could I do to avoid being a bottleneck? Go, a language designed at Google, would have been an option, as it allows running code without installing anything on anyone's computer, Mac, Windows, or Linux. And MessageBird provides a Go API! The only issue: I've never programmed in Go, and my calendar only has that so many openings!

### AI-enhanced development

However, I read a [post] from the co-founder of Django on how he became more ambitious with his projects thanks to AI-enhanced development. I had to try, so I started with the MessageBird Go API documentation:

```golang
client := messagebird.New("YOUR_ACCESS_KEY")
message, err := sms.Create(
  client,
  sender,
  []string{receiver},
  message_text
)
```

The code is similar to the Python snippet, with some puzzling syntax such as `[]string{receiver}`, and that `err` lying around. I decided to give it a Go!

### Human-crafted architecture

First of all, how should the program be structured? I didn't bother asking ChatGPT, as just explaining the requirements was probably longer than just doing it. 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">To replace programmers with AI, clients will need to accurately describe what they want.<br><br>We’re safe.<a href="https://twitter.com/hashtag/softwareengineering?src=hash&amp;ref_src=twsrc%5Etfw">#softwareengineering</a> <a href="https://twitter.com/hashtag/programming?src=hash&amp;ref_src=twsrc%5Etfw">#programming</a></p>&mdash; Dr. Milan Milanović (@milan_milanovic) <a href="https://twitter.com/milan_milanovic/status/1622127889108926466?ref_src=twsrc%5Etfw">February 5, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


To start, I assume my data will look like Table 1 (but without headers) and be stored in a CSV file.

<table style="border-collapse: collapse; width: 50%; margin-left: auto; margin-right: auto;">  
<caption style="font-size: 1.1em;">Table 1: The CSV file containing the name and phone number of our customers</caption>
  <thead>  
    <tr>  
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left; background-color: #f2f2f2;">name</th>  
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left; background-color: #f2f2f2;">phone</th>  
    </tr>  
  </thead>  
  <tbody>  
    <tr>  
      <td style="border: 1px solid #ddd; padding: 8px; text-align: left;">John</td>  
      <td style="border: 1px solid #ddd; padding: 8px; text-align: left;">12345</td>  
    </tr>  
    <tr>  
      <td style="border: 1px solid #ddd; padding: 8px; text-align: left;">Mary</td>  
      <td style="border: 1px solid #ddd; padding: 8px; text-align: left;">54321</td>  
    </tr>  
  </tbody>  
</table>

Afterward, as the documentation showed, I need to create the MessageBird `client` and pass it the access key. Then, I should read the CSV file with the records, split each record by name and phone number, and, for each record, send an SMS. I could address customers by their first names to make it more personal.

The access key, though, is a secret and shouldn't be in the code I write. How should I approach this in Go?

### Bringing ChatGPT for a spin

Time to try out our new AI overlord:

> Q: What's the proper way to pass secrets to a Golang program?

> A: Using environment variables is the proper way to pass secrets, such as API keys or sensitive configuration data, to a Golang program. 

The code suggested:

```golang
// GENERATED BY CHATGPT, EXCLUDING THIS COMMENT
package main  
  
import (  
    "fmt"  
    "os"  
)  
  
func main() {  
    secretKey := os.Getenv("MY_SECRET_KEY")  
    if secretKey == "" {  
        fmt.Println("MY_SECRET_KEY environment variable is not set")  
        os.Exit(1)  
    }  
  
    fmt.Println("My secret key is:", secretKey)  
}  
```

Great! Now I can use the info from the MessageBird docs to create the client with the `secretKey`. Next up is reading a CSV file and looping over each row:

> Q: How do I read a CSV file in go and then loop over each record?

```golang
// GENERATED BY CHATGPT, EXCLUDING THIS COMMENT
package main  
  
import (  
	"encoding/csv"  
	"fmt"  
	"log"  
	"os"  
)  
  
func main() {  
	// Open the CSV file  
	file, err := os.Open("data.csv")  
	if err != nil {  
		log.Fatal("Error opening the file:", err)  
	}  
	defer file.Close()  
  
	// Create a new CSV reader  
	reader := csv.NewReader(file)  
  
	// Read the header line (if present)  
	header, err := reader.Read()  
	if err != nil {  
		log.Fatal("Error reading the header:", err)  
	}  
	fmt.Println("Header:", header)  
  
	// Loop over the records  
	for {  
		record, err := reader.Read()  
		if err != nil {  
			break  
		}  
		fmt.Println("Record:", record)  
	}  
```

That's great, but I need to access individual fields in the record. 

> Q: How do I split a record, coming out of reader.Read() in go, so that I can do something with each field?

A long answer came out, with the proper solution buried in the code, but not exactly what I asked. Also note that I can use *golang*, *go*, or *Go* interchangeably. The AI knows.

> A: When you read a record using reader.Read() from the encoding/csv package in Go, it returns a slice of strings
> 
```golang
// GENERATED BY CHATGPT, EXCLUDING THIS COMMENT
// stuff omitted for brevity
for {  
  record, err := reader.Read()  
  if err != nil {  
    break  
  }  
  // Process each field in the record  
  for i, field := range record {  
    fmt.Printf("Field %d (%s): %s\n", i, header[i], field)  
  }  
  fmt.Println("----")  
	}  
```

Ah, `header[i]` is how you do it. Great! Now I have all the ingredients, and I just need to create a function that takes the name, phone, a MessageBird client, a sender and sends the SMS!

> Q: How do I create a function in go?

```golang
// GENERATED BY CHATGPT, EXCLUDING THIS COMMENT
func add(a int, b int) int {  
	sum := a + b  
	return sum  
}
```

OK, it seems easy. Now my turn to code:

```golang
func sendMessage(client, text str, sender str, recipient str, name str) {
   	msg, err := sms.Create(
		client,
		sender,
		[]string{recipient},
		text,
		nil,
	)
	if err != nil {
		log.Println(err)
	}
}
```

OK, almost there. This code has two problems:

1. `client` has no type in the function signature, and Go doesn't like that!
2. I want to interpolate the `name` variable into the `text` variable to personalize the message

### The finishing touches

> Q: How do I find out the type of a go variable?

ChatGPT replied: `fmt.Printf("Type of var1 (%v) is: %T\n", var1, var1)`. Upon running the snippet, I got `*messagebird.Client`, which is what the doctor ordered. Later, after I set up [NeoVim] with the Go Language Server protocol ([LSP]), the editor would conveniently suggest the type automatically.

> Q: How do I do string interpolation in go?

The long answer contained the info I needed: `message := fmt.Sprintf("%s is %d years old and lives in %s.", name, age, city)`. So my code to send the SMS is now:

```golang
func sendMessage(client *messageBird.Client, text str, sender str, recipient str, name str) {
  msg, err := sms.Create(
    client,
    sender,
    []string{recipient},
    fmt.Sprintf(text, name)  // text should have a single %s where the `name` should be!!!!
  )
  if err != nil {
  log.Println(err)
 }
}
```

### Putting it all together

I assembled the puzzle with all the pieces from ChatGPT and the MessageBird's documentation:

```golang
package main

import (
	"encoding/csv"
	"fmt"
	"log"
	"os"

	messagebird "github.com/messagebird/go-rest-api"
	"github.com/messagebird/go-rest-api/sms"
)

func sendMessage(client *messagebird.Client, text string, sender string, recipient string, name string) {
	msg, err := sms.Create(
		client,
		sender,
		[]string{recipient},
		fmt.Sprintf(text, name), // text should have a single %s where the `name` should be!!!!
		nil,
	)
	if err != nil {
		log.Println(err)
	}
	log.Println(msg)
}

func main() {
	secretKey := os.Getenv("MESSAGE_BIRD_API")
	if secretKey == "" {
		fmt.Println("MESSAGE_BIRD_API environment variable is not set")
		os.Exit(1)
	}
	client := messagebird.New(secretKey)

	// Open the CSV file
	file, err := os.Open("data.csv")
	if err != nil {
		log.Fatal("Error opening the file:", err)
	}
	defer file.Close()

	// Create a new CSV reader
	reader := csv.NewReader(file)

	// Loop over the records
	for {
		record, err := reader.Read()
		if err != nil {
			break
		}
		sendMessage(client, "Hey %s", "Xebia", record[1], record[0]) // assuming the csv file has the format `name, phone`
	}
}
```

This is all there is to it. Time to run it?

> Q: How do I run a go program?

> A: `go run main.go`

Well, that didn't work ChatGPT. My console complained about a missing `go.mod` file.

> Q: How do I create the go.mod file in a go project?

The answer was `go mod init example.com/myproject`. Afterward, I got some errors about missing modules, but Go was so kind to tell me how to fix them:

```
go get github.com/messagebird/go-rest-api/
go get github.com/messagebird/go-rest-api/sms
```

Having a working project is terrific because I can start adding new functionality one piece at a time afterward. Since then, I've added configuration management, schema flexibility, and templating. In the spirit of open source, I've put the repository on [GitHub] for everyone's benefit. I still have a couple of TODOs (adding extra documentation and tests), and I want to leverage Copilot to tackle them. More on a future post.

### Looking forward

The speed at which I could get working code in a new language was astonishing, and I think this will have two main consequences. First, if all your coding is made of small repetitive pieces—rewriting a SQL script from one SQL dialect to another, documentation, creating simple APIs on top of databases—learn to master LLMs prompting, or someone else will take your place. There is simply no way ChatGPT, or an LLM fine-tuned to this task can't figure out what you're doing with minimal oversight. Second, deft programmers starting or experimenting in a new area will have fun. You can do the high-level thinking, let LLMs handle time-consuming, simple problems, check their output— as, at the moment, they do not **understand** what you're telling them— and then assemble the pieces.


<br/>
<br/>

<section class="w-full has-list-check pb-sm bg-grey-100 pb-sm relative px-2 md:px-0">
    
        <div class="cta-wave-top w-full relative -mt-md -mb-md data-appear">
            <img class="w-full" src="https://xebia.com/wp-content/themes/xebia-theme/images/cta-wave.svg" alt="">
        </div>
    <!-- 1 text + img -->
        <div class="container relative z-20 md:flex pb-sm ">
            <div class="data-appear-group w-full md:w-2/3 pr-sm">
                <h2 class="mt-0 text-2xl md:text-3xl pb-0 text-purple"> <div id="hs_cos_wrapper_widget_1683625652060_" class="hs_cos_wrapper hs_cos_wrapper_widget hs_cos_wrapper_type_inline_text" data-hs-cos-general-type="widget" data-hs-cos-type="inline_text" data-hs-cos-field="section_title">Copilot Training</div></h2>
                <div class="text-base md:text-xl"> <div id="hs_cos_wrapper_widget_1683625652060_" class="hs_cos_wrapper hs_cos_wrapper_widget hs_cos_wrapper_type_inline_rich_text" data-hs-cos-general-type="widget" data-hs-cos-type="inline_rich_text" data-hs-cos-field="section_text"><span>If you're looking for help to get started, we just launched a training program to help you leverage LLMs to be more effective while coding. I'll be there too, so I can learn from the best what I'm still missing to ride this wave even higher and faster.</span></div> </div>               
            </div>
            <div class="md:px-md md:pt-sm w-full md:w-1/3">
                <div class="pb-sm"><a href="https://xebia.com/academy/en/training/copilot" class="btn-outline">More info</a></div>
            </div>
        </div>
        <div class="cta-wave-bottom w-full absolute bottom-0 data-appear">
            <img src="https://xebia.com/wp-content/themes/xebia-theme/images/cta-wave-bottom.svg" alt="">
        </div>
</section>


[post]: https://simonwillison.net/2023/Mar/27/ai-enhanced-development/
[GitHub]: https://github.com/gglanzani/bulk-messagebird-sender/
[program]: https://xebia.com/academy/en/training/copilot
[headwinds]: https://messagebird.com/blog/ceo-robert-vis-email-to-messagebird-employees
[NeoVim]: https://neovim.io
[LSP]: https://langserver.org/
