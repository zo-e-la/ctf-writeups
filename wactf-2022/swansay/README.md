# swansay (200pts) 
**TYPE: WEB APPLICATION**
## Brief 
Introducing... Swansay! It's like cowsay... but better ;)
## Process
#### 1. Opened link, presented with this guy:
![swan_2](https://user-images.githubusercontent.com/30396122/205581081-dfef3e2b-674f-4ce5-84c3-7cd4e0e3dff2.png)
Input string into text box and swan will swansay. 
##
#### 2. Tried to inject '.' and then "." into textbox to see what the response is: 
"." entry came back with an eval() error. 
> eval() is evil
as a wise man once said... 

Basically the text input of the user shoud be santised properly, otherwise malicious code can be executed.  
##
#### 3. Next step was to figure out what language is used, by looking at source code: 
Found that the source code includes some interesting packages including turbo rails. 
Assuming turbo rails is association with Ruby on Rails, I did some research. 
I found the package they were using on github: 
* [Turbo Rails](https://github.com/hotwired/turbo-rails) 
##
#### 4. Wrote inject in Ruby
```
"+[random value for test]+"
```
Returned a different error asking for a valid parameter. Good sign that the language we are using is correct.  
##
#### 5. Wrote command inject
```
"+`[COMMAND]`+"
```
Found out it does not like backticks (so there is some filtering going on), because it returned a new error message unrelated to eval() or parameters. 
##
#### 6. We need to find a way to bypass the filtering 
First I thought of URL encoding the backticks, however this didn't work. 
Then I research a couple of other ways of encoding backticks. 
After these failed I thought, I could encode the backticks and then decode it with a common function used in Ruby (that would be imported in the program). 
##
#### 7. Tried encoding string with base64 and then decoding with Ruby's [Base64.decode()](https://ruby-doc.org/stdlib-2.5.3/libdoc/base64/rdoc/Base64.html) function
The string below is \`ls\` in base64 
```
"+ Base64.decode64('YGxzYA==') +"
```
Results: 

![swan_3](https://user-images.githubusercontent.com/30396122/205585650-53c78f41-9716-44a1-9c67-524d2eaf0909.png)

Thoughts: Okay, backticks are bypassing the filtering however it is processing the base64 payload as a string not a command. 
##
#### 8. Researched a function in ruby that makes strings into a command
Found this [stackoverflow post](https://stackoverflow.com/questions/24685037/convert-string-to-a-function-in-ruby-on-rails) on instance_eval() in Ruby. 
Also found [this article](https://medium.com/rubycademy/ruby-instance-eval-a49fd4afa268) with more information. 
##
#### 9. Added function to payload 
The string below is \`id\` in base64 
```
"+ instance_eval(Base64.decode64('YGlkYA=='))  +"
```
Results: 

![swan_4](https://user-images.githubusercontent.com/30396122/205587505-26c7335e-d53e-4719-938f-e5abde105a6c.png)
##
#### 10. Now simply investigate the files with \`ls\` and then enter \`cat [FLAG FILE NAME]\` encoded 
The string below is \`cat [FLAG FILE NAME]\` in base64 
```
"+ instance_eval(Base64.decode64('YGNhdCBmbGFnX2VhMjY4YmFkNzJhLnR4dGA='))  +”
```
##
#### 11. Flag!
![swan_flag 182025](https://user-images.githubusercontent.com/30396122/205588102-c3d42197-751c-4532-955a-9a7fd75d40f2.png)
```
WACTF{3V41_15_t7uLy_evIl_bf3dbozaeb7b2}
```
##
## Conclusion 
This was an awesome CTF and I enjoyed it, mainly because I was pretty much on the right track the whole time. Plus I would classify this as my first completely solo CTF solve! 
