# HTML5 Sketch Tutorial
This is where I will tell you how to make the sketch
<br>But what if you are too lazy? vist step 10 for the FULL code
## What will it look like?
![image](https://user-images.githubusercontent.com/92959844/155481538-3962eb1e-4b26-49bf-9121-86b289119df8.png)
## Step 1: Create the "index.html" file
First off, we need an index.html file, put these HTML code on that file:

    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <link rel="stylesheet" href="style.css">
            <title>JavaScript Drawing APP</title>
        </head>
        <body>
            <canvas id="canvas"></canvas>

            <script src="main.js"></script>
        </body>
    </html>
## Step 2: Create the "style.css" file
Now that we have the index, we will need a basic css file (name it "style.css")

    *{
       margin: 0;
       padding: 0;
    }
## Step 3: Create the "main.js" file
Our index is now basiclly styled, we need to put some functionality (name it "main.js")

    const canvas = document.getElementById("canvas")
    canvas.height = window.innerHeight
    canvas.width = window.innerWidth

    // ctx is the context of our canvas
    // we use ctx to draw on the canvas
    const ctx = canvas.getContext("2d")

    // lets create a rectangle for testing purposes
    ctx.fillStyle = "red"
    ctx.fillRect(100, 100, 100, 100)
## Step 4: Remove the red square
If you take a look at what you have made so far, it should have a red square, but we need to remove it

    //Remove these code
    ctx.fillStyle = "red"
    ctx.fillRect(100, 100, 100, 100)
    
    //Replace it with this
    window.addEventListener("mousemove", (e) => {
    console.log("Mouse X: " + e.clientX)
    console.log("Mouse Y: " + e.clientY)
    })
## Step 5: Create the main function
Now that we have removed that naughty red square, we also need keep track of the previous mouse position
and draw a line from the previous mouse position to current mouse position, so:

    //Remove these code
    window.addEventListener("mousemove", (e) => {
    console.log("Mouse X: " + e.clientX)
    console.log("Mouse Y: " + e.clientY)
    })
    
    //Replace it with
    let prevX = null
    let prevY = null

    // How thick the lines should be
    ctx.lineWidth = 5

    window.addEventListener("mousemove", (e) => {
    // initially previous mouse positions are null
    // so we can't draw a line
    if(prevX == null || prevY == null){
          // Set the previous mouse positions to the current mouse positions
          prevX = e.clientX
          prevY = e.clientY
          return
      } 

      // Current mouse position
      let currentX = e.clientX
      let currentY = e.clientY

      // Drawing a line from the previous mouse position to the current mouse position
      ctx.beginPath()
      ctx.moveTo(prevX, prevY)
      ctx.lineTo(currentX, currentY)
      ctx.stroke()

      // Update previous mouse position
      prevX = currentX
      prevY = currentY
    })
## Step 6: Make the drawing controlable
We are almost done, but right now, we need to make sure that we can control what we are drawing, so replace all your JS codes with this one:

    const canvas = document.getElementById("canvas")
    canvas.height = window.innerHeight
    canvas.width = window.innerWidth

      const ctx = canvas.getContext("2d")

    let prevX = null
    let prevY = null

    ctx.lineWidth = 5

    let draw = false

    // Set draw to true when mouse is pressed
    window.addEventListener("mousedown", (e) => draw = true)
    // Set draw to false when mouse is released
    window.addEventListener("mouseup", (e) => draw = false)

    window.addEventListener("mousemove", (e) => {
    // if draw is false then we won't draw
    if(prevX == null || prevY == null || !draw){
        prevX = e.clientX
        prevY = e.clientY
        return
    }

    let currentX = e.clientX
    let currentY = e.clientY

    ctx.beginPath()
    ctx.moveTo(prevX, prevY)
    ctx.lineTo(currentX, currentY)
    ctx.stroke()

    prevX = currentX
    prevY = currentY
    })
## Step 7: Make the color changable
We are done with the main functionality, but we want to make the brush changable, so put these final HTML code right below the "canvas" tag (and right above the "script" tag)

    <div class="nav">
        <div class="clr" data-clr="#000"></div>
        <div class="clr" data-clr="#EF626C"></div>
        <div class="clr" data-clr="#fdec03"></div>
        <div class="clr" data-clr="#24d102"></div>
        <div class="clr" data-clr="#fff"></div>
        <button class="clear">clear</button>
        <button class="save">save</button>
    </div>
## Step 8: Update the CSS
We almost forgot about the CSS, so place these CSS codes on style.css

    .nav{
        width: 310px;
        height: 50px;
        position: fixed;
        top: 0;
    left: 50%;
        transform: translateX(-50%);
        display: flex;
        align-items: center;
        justify-content: space-around;
        opacity: .3;
        transition: opacity .5s;
    }
    .nav:hover{
        opacity: 1;
    }

    .clr{
        height: 30px;
        width: 30px;
        background-color: blue;
        border-radius: 50%;
        border: 3px solid rgb(214, 214, 214);
        transition: transform .5s;
    }
    .clr:hover{
        transform: scale(1.2);
    }
    .clr:nth-child(1){
        background-color: #000;
    }
    .clr:nth-child(2){
        background-color: #EF626C;
    }
    .clr:nth-child(3){
        background-color: #fdec03;
    }
    .clr:nth-child(4){
        background-color: #24d102;
    }
    .clr:nth-child(5){
        background-color: #fff;
    }

    button{
        border: none;
        outline: none;
        padding: .6em 1em;
        border-radius: 3px;
        background-color: #03bb56;
        color: #fff;
    }
    .save{
        background-color: #0f65d4;
    }
## Step 9: Make the "clear" and "save" usable
This is the final step in this tutorial, so replace all your JS codes agian with this final JS code (styled/formmated JS)

    const canvas = document.getElementById("canvas")
    canvas.height = window.innerHeight
    canvas.width = window.innerWidth

    const ctx = canvas.getContext("2d")

    let prevX = null
    let prevY = null

    ctx.lineWidth = 5

    let draw = false

    let clrs = document.querySelectorAll(".clr")
    clrs = Array.from(clrs)
    clrs.forEach(clr => {
        clr.addEventListener("click", () => {
            ctx.strokeStyle = clr.dataset.clr
        })
    })

    let clearBtn = document.querySelector(".clear")
    clearBtn.addEventListener("click", () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height)
    })

    // Saving drawing as image
    let saveBtn = document.querySelector(".save")
    saveBtn.addEventListener("click", () => {
        let data = canvas.toDataURL("imag/png")
        let a = document.createElement("a")
        a.href = data
        // what ever name you specify here
        // the image will be saved as that name
        a.download = "sketch.png"
        a.click()
    })

    window.addEventListener("mousedown", (e) => draw = true)
    window.addEventListener("mouseup", (e) => draw = false)

    window.addEventListener("mousemove", (e) => {
        if(prevX == null || prevY == null || !draw){
            prevX = e.clientX
            prevY = e.clientY
            return
        }

        let currentX = e.clientX
        let currentY = e.clientY

        ctx.beginPath()
        ctx.moveTo(prevX, prevY)
        ctx.lineTo(currentX, currentY)
        ctx.stroke()

        prevX = currentX
        prevY = currentY
    })
## Step 10: Finishing what we've done
We finally made our HTML draw tool, if you have'nt understood or you got stuck somewhere, you can visit the full code at
[codepen.io](https://codepen.io/michael-angelo-vicera/pen/qBVyNGY)
<br>If there are any errors, feel free to report it to me
