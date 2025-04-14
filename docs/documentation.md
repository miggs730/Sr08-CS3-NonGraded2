# Documentation ✨
**2nd Graded Exercise**<br>
*Lawrence Miguel Cereñado*

Hello there! This is a documentation.md file containing the steps that I have done for the club signup website to work! This also contains the references I used for making this amazing webpage:> 💓 

## 1️⃣ GET Route for the  join.hbs file

Firstly, we have to resolve some routes. One of those routes involves our join.hbs file.

This get route will help us render our hbs file to display on our web browser! Here's the code I managed to devise to do this get route:
``` javascript
app.get('/join', (req, res) => {
  console.log("Form loaded successfully") 
  try {
    const clubs=  JSON.parse(fs.readFileSync(clubFilePath));
    const clubsList = Object.keys(clubs);
    console.log(clubsList);
    res.render('join',{clubsList});
  }
  catch (error) {
    console.log('Error reading clubs file', error);
    res.status(500).send('Error loading user data');
  }
});
```

Additionally, earlier in the file, I declared a constant named clubFilePath using some FS methods to simplify our path and use it anywhere without writing the whole path. This is an alternative to passing the clubsArray variable.
``` javascript
const clubFilePath = path.join(__dirname, 'data', 'clubs.json');
```

❓  **What does these code do?**
<ol>
<li>This code sets a route to '/join', which is the other Handlebars file we would be using for forms and getting data from the user. </li>
<li>We use the try and catch methods which would help us debug our code for any errors if it happens in any case. We also have terminal logs to make sure everything is working alright. </li>
<li> We parse the JSON object from our clubs.json file then obtain an array from it named clubList  which contains an array of all the clubs' names we would be using. We use Object.keys() for this.</li>
<li> Finally we render the file along with the object for the HBS file to process later on! </li>
</ol>

## 2️⃣ Selecting Options in our join.hbs file
Now that we have our file loaded, we have to address issues in our hbs file. Notice that our select option doesn't work, so we have to address that issue. We can easily fix that through some HBS code using templates! 

``` handlebars
<label for="organization">Preferred Organization / Club:</label>
<select id="preferredClub" name="preferredClub" required>
    <option value = ""> Select a club you prefer! </option>
    {{#each clubsList}} 
    <option value = "">{{this}}</option>
    {{/each}}
</select>
```
Now that we have our file loaded, we have to address issues in our hbs file. Notice that our select option doesn't work, so we have to address that issue. We can easily fix that through some HBS code using templates! 

We use the {{#each}} template in HBS for us to iterate over the clubsList array that we have obtained in Step 1. Then we use {{this}} for the option to show up!

## 3️⃣ POST Route for the join.hbs file 
//
# 📖 References
as of time 3:50AM, none so far.


https://www.youtube.com/watch?v=JB7YD7OKm5g
