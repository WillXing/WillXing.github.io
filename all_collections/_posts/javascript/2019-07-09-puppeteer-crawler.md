---
layout: post
title:  "Spider assistant to find a job (Heroku + Puppeteer)"
author: "Will Xing"
categories:
   - javascript
   - blog
date: 2019-07-09T00:00:00Z
---

My wife have been a full time house wife for almost one year, and during that time we move from China to New Zealand.

Now she want to find a job, but it's really hard for her to return to work force since she doesn't have a hard skill and a career in New Zealand is totally a new start for her, also she need more time to improve her English.

But we don't want to wait, she's tired about looking after kid, and our son is way too energetic.

She was doing recruiting, marketing and admin back in China, she want to do the same thing. So we start to looking for a job every where and she is sending her CV every where, most of the time she have no response, and the rest of few times she will get a auto reply email or reject email 💔.

I was thinking about this while I am sleeping in past few days, thinking about the reasons make it hard to find a job for her and how can I help her.

There are following reasons make it hard to find a job:
1. She doesn't have any work experience here.
2. She doesn't have some hard skill eg. Programming, Accounting, Hair cutting...
3. She needs to improve her English.
3. Not too much company want to hire a recruiting, marketing or admin, if there is one there will be too many people waiting fighting for the position.

And how can I help with this?
1. This one can only change once she find a job.
2. This is a long term things to do, I can give suggestion maybe...
3. I can encourage her to speak more and learn harder!
4. **This one I think I can help with it now...**

## Problem

The problem is very simple: **Small mount of opportunity, big mount of competitor**. Company here only hire one staff for the position. She usually never get any response it's because the employer already got many CVs before she apply, and
```
usually 1 out of 10 candidates
will get the offer.
                    --- Isaid
```

## Solution

So it's straight forward to me now, I need to help her to keep an eye on the recruiting ads, and once I find the new one, give the link and information to here ASAP.

Not manually, because I can't always check it, I got work to do.

## Commit

First thing is to find a website, we found a Chinese forum is pretty good, it's very active, and looks like no anti-spider.

### Puppeteer

For the scrawl framework, I am using [puppeteer](https://github.com/GoogleChrome/puppeteer), which is a

    Puppeteer is a Node library which provides a high-level API to control Chrome or Chromium
    over the DevTools Protocol. Puppeteer runs headless by default, but can be configured to
    run full (non-headless) Chrome or Chromium.

Actually it's usually using to do e2e test, but it's also using to do web scrawl. It's actually pretty good to be a scrawl framework, because it's able to lunch and operate a Chrome browser, this can make it not easy to be recognise by anti-spider.( never tried not easy web site yet. )

And puppeteer is easy to coding, if you know JavaScript, then it's easy as. Basically you lunch a browser and then you go to a page like this:

```js
const browser = await puppeteer.launch();
const page = await browser.newPage();
await page.goto('http://example.com/forum.php?fid=1&page=1');
```

Now the browser is in that page, so next thing is just to get the element you need, it's like all other framework, support css selector to help you find the element, you just need to put the query in the callback of `page.evaluate`, like following are the simple example:

```js
const jobName = await page.evaluate(() => {
  return document.querySelector('table > tbody > tr:nth-child(3) > td.td-content').innerHTML;
});
```

Also it's like jQuery you can query in an element, so like this:

```js
const job = await page.evaluate(() => {
  return [...document.querySelectorAll('table tbody > tr')].map(post => {
    return {
      name: post.querySelector('table > tbody > tr:nth-child(3) > td.td-content').innerHTML;
      salary: post.querySelector('table > tbody > tr:nth-child(4) > td.td-content').innerHTML;
    }
  })
});
```

### Node-Cron (Crontab)

After done the crawling code, looks like I need to run this every 30mins to get the job list, so here's why I need [node-cron](https://www.npmjs.com/package/node-cron). It's just a timer, if you do `setInterval(fun, 1800000)` it will also work the same. I like this one because it's base on the real time, like if I say `*/30 * * * *`, it will run at 00:30, 01:00, 01:30... and always like this, no matter when you restart it, and also I don't need to calculate mm. You may found it's not easy to set the schedule, but here is a [tool can help you](https://crontab.guru/) calculate your schedule string. Then the code is like following:

```js
cron.schedule('*/30 * * * *', async () => {
  await crawlData();
});
```

### Data Saving

I am using [wilddog](wilddog.com) to save my data, and by the REST support, it's so easy to use. Basically just `POST` the data to the endpoint, and `GET` to fetch data from same endpoint.

### Heroku

Of cause I can't open my machine 24 hours to run the scheduled job to crawl the data. So I deploy to [Heroku](heroku.com), it's free and integrate very well with CLI, also it's support NodeJS app so good. What I need to do is just to put the start command in the npm start script like this:

```json
"scripts": {
  "start": "node index.js",
},
```

Then connect the GIT repo to heroku app and click deploy. Heroku will deploy your code and compile it, then start it.

When running puppeteer in the heroku environment I encounter with an error which said `Failed to launch chrome!`, if Chrome can't lunch, puppeteer code can't work. The solution is simple, but took me some time to find it. The reason is because heroku for NodeJS app doesn't have the dependencies that Chrome need, what need to do for that, is add a buildpacks for [heroku puppeteer](https://github.com/jontewks/puppeteer-heroku-buildpack) to your app:

```bash
heroku buildpacks:set https://github.com/jontewks/puppeteer-heroku-buildpack.git
```

And also remeber to start your puppeteer browser in headless and no-sandbox mode:

```js
const browser = await puppeteer.launch({
  headless: true,
  args: ['--no-sandbox', '--disable-setuid-sandbox']
});
```

Now the heroku should be working fine!

### Static display (Express + Pug)

My wife can't login to the server to check the job, otherwise she will be able to find a job!

So I use [express](https://expressjs.com/) and [pug](https://pugjs.org) to do the server side render a static web page for her (It's safe! No one can see my not secured API).

Express is a web framework for node, and pug is a template framework which using jade. Express support pug very well so code is simple:

```js
app.set('view engine', 'pug');
app.use("/static", express.static(path.join(__dirname, "public"))); //This is the folder hosting my css file
app.get('/', async (req, res) => {
  const response = await axios.get('https://example.com/jobs.json');
  const jobs = response.data;
  res.render('template', { jobs })
});

app.listen(port, () => console.log(`App listening on port ${port}!`));
```

And in pug template just do:

```jade
doctype html
html
  head
    link(href='/static/main.css', rel='stylesheet')
body
  table
    thead
      tr
        th Num
        th Date
        th Name
        th Salary
    tbody
      -var num = 0
      each val, index in jobs
        tr
          td= ++num
          td= val.date
          td
            a(href=val.link)=val.name
          td= val.salary
```

### So here is how the website looks like

![shot](/assets/images/jobdata.png)

## Next

Probably will send mail to the employer automatically :).