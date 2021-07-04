# reactColorShader
This React app takes in a user color and returns an assortment of that colors shades and tints. Also allows for C&amp;P to the clipboard. To get the colors, an external library is used:
values.js-(https://github.com/noeldelgado/values.js). 

A brief overview of the pertinent React code is given below. 

To begin the states were setup along with the first part of the return including the handleSubmit() function which is fired off from the form.
```React
function App() {
  const [color, setColor] = useState("");
  const [error, setError] = useState(false);
  const [list, setList] = useState(new Values("#253785").all(10));

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('test');
    }
  };

  return (
    <>
      <section className="container">
        <h3>color generator</h3>
        <form onSubmit={handleSubmit}>
          <input
            type="text"
            value={color}
            onChange={(e) => setColor(e.target.value)}
            placeholder="#f15025"
            
          />
          <button className="btn" type="submit">
            submit
          </button>
        </form>
      </section>
      <section className="colors">
      </section>
    </>
  );
}
```

After the initial setup and the console.log is checked then the functionality is started including working with the values.js library.
```React
  const handleSubmit = (e) => {
    e.preventDefault();
      let colors = new Values(color).all(10);
      console.log(colors
   }
   ```

To handle errors in case of invalid input a try/catch block is used.
```React
  const handleSubmit = (e) => {
    e.preventDefault();
    try {
      let colors = new Values(color).all(10);
    } catch (error) {
      console.log(error);
      setError(true);
    }
  };
  ```
  
  To render the error the setError is used inside of the <input> to change the class which creates a red border around the input box with an invalid submission.
  ```React
            <input
            type="text"
            value={color}
            onChange={(e) => setColor(e.target.value)}
            placeholder="#f15025"
            className={`${error ? "error" : null}`}
          />
      ```
      
      Now that the basic input functionality is done either an [] or objects or an error is returned. Now work begins on the data list which comes from new Values().
      ```React
        const handleSubmit = (e) => {
    e.preventDefault();
    try {
      let colors = new Values(color).all(10);
      setList(colors);
    } catch (error) {
      console.log(error);
      setError(true);
    }
  };
  ```
  
  
  Now the retreived colors from new Value() are iterated over.
  ```React
        <section className="colors">
        {list.map((color, index) => {
          return <SingleColor key={index} {...color} index={index} />;
        })}
      </section>
      ```
      
      
      Inside of SingleColor.js the required props are brought in from App.js.
      ```React
      const SingleColor = ({ rgb, weight, index, hexcolor }) => {}
      ```
      
      What is required are the rgb values from the returned data objects. To make each tile background colored the rgb prop will be used as well as the weight property.
      ```React
      const SingleColor = ({ rgb, weight, index, hexcolor }) => {
  const [alert, setAlert] = useState(false);
  const bcg = rgb.join(",");

  return (
    <article
      className={`color${index > 10 && "color-light"}`}
      style={{ backgroundColor: `rgb(${bcg})` }}
    >
    </article>
  );
};
```

Now that each tile has a unique background color they need to have their hex and weights displayed as well. For this to work the rgbToHex() function from utils.js is imported and used.
```React
  return (
  <article class=...
      <p className="percent-value">{weight}%</p>
      <p className="color-value">{hex}</p>
      {alert && <p className="alert">Copied to clipboard</p>}
    </article>
  );
   const hex = rgbToHex(...rgb);
   ```
   
   Now that both weight and hex are displayed on each tile the tiles should be ordered from light to base color to darkest. This is done by using the index value.
   ```React
       <article
      className={`color${index > 10 && "color-light"}`}
      style={{ backgroundColor: `rgb(${bcg})` }}...
      ```
      
      To make the value accessible:
      ```React
       const hexValue = `#${hexcolor}`;
       ```
       
       Now the copy to clipboard functionality is set up.
       ```React
       ...
             <p className="percent-value">{weight}%</p>
      <p className="color-value">{hex}</p>
      {alert && <p className="alert">Copied to clipboard</p>}
    </article>
    ```
    
    Now to set up the functionality to work on click:
    ```React
        <article
      className={`color${index > 10 && "color-light"}`}
      style={{ backgroundColor: `rgb(${bcg})` }}
      onClick={() => {
        setAlert(true);
        navigator.clipboard.writeText(hexValue);
      }}
    >
    ```
    
    Now the copied to clipboard message is hidden after 3 seconds.
    ```React
      useEffect(() => {
    const timeout = setTimeout(() => {
      setAlert(false);
    }, 3000);
    return () => clearTimeout(timeout);
  }, [alert]);
  ```
  
  Then a default color is created to be displayed upon page loading.
  ```React
    const [list, setList] = useState(new Values("#253785").all(10));
    ```
    
    Lastly, render to the screen.
    ```React
    ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```


***End walkthrough
    
  
    
    
       
      
   


      
      
  
  
