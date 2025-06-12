---
layout: page
title: QR Code Generation
description: No websites, just Python!
img: assets/img/animated_qrcode.gif
importance: 5
category: Personal
related_publications: true
code_url: https://colab.research.google.com/drive/1YJWbboQpqRUVI2qyx852cIaqJ9feHG_k?usp=sharing
code_label: Open in Colab
---
<hr>
<p>
    This is a rather short snippet of how to use Python to create your own QR Codes. While the codebook provided is relatively simple and it will not run unless you import it into a different environment, you can follow a more complex guide by
        <i>
            <a href="https://realpython.com/python-generate-qr-code/" target="_blank" rel="noopener">
            clicking on this link. 
            </a>
        </i> 
</p>
<b>This is just a very simple way to demonstrate the versatility behind python.</b>
<p>
    I wanted to create a QR code for my portfolio, but I was not satisfied with all the websites asking me to submit my email address or provide them with payment information. Luckily, there are a myriad of libraries available within python that allow us to do such things free of charge.
</p>
<hr />
<p>
    You are going to require the following:
        <ul>
            <li>Access to a code editing software. I personally prefer <a href="https://code.visualstudio.com/docs/setup/setup-overview" target="_blank" rel="noopener">
                VS Code,</a> but it is up to you.  </li>
            <li>Run the following command in your terminal:</li>
        </ul>
</p>
```bash
pip install segno
```
<hr />
<p>
    The code is relatively simple! I have also gone ahead and simplified it into a function so it can easily be implemented across other projects
</p> 
<details>
  <summary>You can click here to see how the <code>generate_qr_code</code> function operates</summary>
  <pre><code>
def generate_qr_code(data, filename='qr_code.png', scale=10):
    """
    Generates a QR code from the given data and saves it to a file.

    :param data: The data to encode in the QR code.
    :param filename: The name of the file to save the QR code image.
    :param scale: The scale factor for the image size.
    """
    qr = segno.make(data)
    qr.save(filename, scale=scale)
    print(f"QR code saved as {filename} (scale={scale})")
  </code></pre>
</details>
<br>
Running the following line of code
```python
# Example usage:
generate_qr_code('https://ilovedogs3003.github.io/', 'my_portfolio_qr_code.png', scale=10)
```
will produce the following image:
<div class="row align-items-center">
    <!-- Image column -->
    <div class="col-md-6 d-flex justify-content-center align-items-center" style="height:100%;">
        <div style="max-width: 400px; width: 100%;">
            {% include figure.liquid 
                path="assets/img/my_portfolio_qr_code.png" 
                title="QR Code for Portfolio" 
                class="img-fluid rounded z-depth-1 w-100" %}
        </div>
    </div>
    <!-- Text column -->
    <div class="col-md-6">
        <br>
        <p>
            If you scan it, it will simply redirect you back to the about page of my portfolio.
        </p>
        <p>
            Additionally, as highlighted by the guide I linked, we can customize it in a varity of ways to meet our needs. Available customizations include, but are not limited to:
            <ul>
                <li>Formatting the border</li>
                <li>Changing the colors of the background </li>
                <li>Changing the colors of the dark squares</li>
                <li>Changing the colors of that dark data modules </li>
                <li>Rotating the QR code</li>
                <li>Adding animations such as GIFs to the QR code</li>
            </ul>
        </p>
    </div>
</div>
<p>
    Considering each customization is rather reiterative and achieved through an additional line of code, I won't model each of the components. However, in honor of the handle for this website, I will leave you with a playful iteration of the QR code you scanned:
</p>
<div class="row">
    <div class="col-sm mt-3 mt-md-0 d-flex justify-content-center">
        <div style="max-width: 400px; width: 100%;">
            {% include figure.liquid loading="eager" path="assets/img/animated_qrcode.gif" title="dog GIF" class="img-fluid rounded z-depth-1" %}
        </div>
    </div>
</div>
