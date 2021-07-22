---
author: John Robin Inston
categories:
- Quantitative Analysis
- Stochastic Processes
- Financial Mathematics
date: "2020-11-30"
draft: false
excerpt: This theme has a form-to-email feature built in, thanks to the simple Formspree
  integration. All you need to activate the form is a valid recipient email address
  saved in the form front matter.
layout: single
links:
- icon: door-open
  icon_pack: fas
  name: website
  url: https://bakeoff.netlify.com/
- icon: github
  icon_pack: fab
  name: code
  url: https://github.com/apreshill/bakeoff
subtitle: Stochastic Processes
tags:
- hugo-site
title: Pricing call options and bonds using Black-Scholes and Vasiciek Models.
---

![University Logo](lancaster-university-logo.png)

---

### Abstract

A project investigating the properties of financial stochastic processes. Specifically, we will consider the implementation of both the Black-Scholes model for determining the price of European call options on stock and the Vasicek model for calculating the future price of bonds. We discuss the background, benefits and drawbacks of both models before conducting our investigation and discussing our findings.

---

```toml
# please replace with a valid Formspree form id or email address
formspree_form_id: johnrobininston@gmail.com
```

Update that file and you're ready to begin receiving submissions. Just submit
the active form for the first time, and complete the email address verification
step with Formspree, and your contact form is live. The next time someone
fills it out, the submission will land in your inbox.

### Multiple Layouts

The files included with the theme have a contact page ready for copy/paste, or
you can type `hugo new forms/contact.md` and you're off to the races. There are two
layouts for `forms` – `split-right`, and `split-left` – you guessed it, one puts
the form on the right and the other on the left. You just fill out the front
matter, and the rest is automatic.

```toml
# layout options: split-right or split-left
layout: split-right
```

![Contact Form Split Right Layout Screenshot](built-in-contact-form-screenshot.png)

Both layouts display the page title and description opposite the form, and you
can also choose to show your social icon links if you have those configured in
the `config.toml` file.
