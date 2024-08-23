# SSTI & CSTI

## SSTI

X-Side Template Injection:-

These Two Types:

1. Client-Side Template Injection
2. Server-Side Template Injection

### 1) Client-Side Template Injection

usually found in JavaScript

for Example

<mark style="color:red;">**\{{\<script> alert(’1773’ \</script>) \}}**</mark>

this payload to GET to Vulnerability SSTI To XSS (Cros-Side scripting)

#### Code Vulnerable CSTI

\<script id="entry-template" type="text/x-handlebars-template"> \<div class="message">\{{userInput\}}\</div> \</script>

#### Exploit CSTI:

```jsx
{{<script>alert('CSTI')</script>}}
```

#### Impact of CSTI

* **Cross-Site Scripting (XSS)**: This is the most common result, where an attacker can execute arbitrary JavaScript in the context of the victim's browser.
* **Data Theft**: Attackers can steal cookies, session tokens, or other sensitive data.
* **Phishing**: Attackers can craft convincing phishing attacks by modifying the DOM to present fake login forms or other malicious content.
* **Remote Code Execution (RCE)**: In rare cases, if the template engine has server-side capabilities or if there are other weaknesses, attackers might be able to achieve RCE.

#### Preventing (Mitigation) CSTI

1. **Input Validation and Sanitization**: Always validate and sanitize user inputs before incorporating them into templates. Use libraries that are designed to safely handle templates and user inputs.
2. **Escape User Inputs**: Ensure that user inputs are properly escaped before being injected into templates. This prevents malicious code from being interpreted as executable code.
3. **Use Trusted Template Engines**: Use well-maintained and trusted template engines that have built-in protections against such injections.
4. **Content Security Policy (CSP)**: Implement CSP headers to restrict the execution of unauthorized scripts, reducing the impact of potential CSTI.

### 2) Server-Side Template Injection

#### Code Vulnerable SSTI

**Jinja2 (Python)**:

```python
from flask import Flask, render_template_string, request
from jinja2 import Template

app = Flask(__name__)

@app.route('/')
def index():
    name = request.args.get('name', 'world')  # Get the 'name' query parameter, defaulting to 'world'
    template = Template('<h2>Hello {{ name }}!</h2>')
    return render_template_string(template.render(name=name))

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

#### **Thymeleaf (Java)**:

```python
@Controller
public class GreetingController {
    @RequestMapping("/greet")
    public String greet(@RequestParam(name="name", required=false, defaultValue="World") String name, Model model) {
        model.addAttribute("name", name);
        return "greet";
    }
}

```

#### Identifying and Exploiting SSTI

1. **Initial Testing**: Inject common template syntax like <mark style="color:red;">**`${7*7}`**</mark><mark style="color:red;">**,**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`{{7*7}}`**</mark><mark style="color:red;">**, or**</mark><mark style="color:red;">** **</mark><mark style="color:red;">**`<%= 7*7 %>`**</mark> into user inputs and observe the output.
2. **Confirming SSTI**: If the application outputs `49` or equivalent computation results, it indicates an SSTI vulnerability.
3. **Payload Crafting**: Once confirmed, craft payloads to execute more complex expressions or functions. For example, in Jinja2

```python
{{ config.items() }}
{{ request.application.__globals__.__builtins__.__import__('os').popen('ls').read() }}
```

#### Mitigation Strategies

1. **Input Sanitization**: Always sanitize and validate user inputs.
2. **Context-Aware Escaping**: Use appropriate escaping mechanisms for the context where user input is included.
3. **Template Engine Configuration**: Configure template engines securely by disabling unnecessary features and restricting execution environments.
4. **Use Safe Substitutions**: Avoid direct inclusion of user inputs in templates. Use safe functions or methods provided by the template engine.
