Design a lottery system for lottery item, include the following part
1. A display screen which show the item name
2. A button which will start the lottery
3. A table to show the current stock situations

Dataset includes:
- An array of objects for each category of prizes
- Each object includes
    - Item name
    - Number

const onlineCoursePrize = [
    {
        name: "Stuffed Animals",
        quantity: 20,
    },
    {
        name: "cups",
        quantity: 20,
    },
    {
        name: "Notebooks",
        quantity: 20,
    }
]

Setup:
1. Find the display element and store it in a variable called display


Function:
A function which will do the following:
    - Set a little bit of loading time for the whole function
    - Clear the screen each time
    - Generate a random number for a random object in the array
    - Display the name of the prize on the screen
    - decrease the number by 1
    - Go through the list of object and check if any of their quantity is equal to 0, if yes, remove the object from the array

FUNCTION lottery():
    display.textContent = "";
    let ranNum = getRandomIntInclusive(0, onlineCoursePrize.length-1);
    let prize = onlineCoursePrize[ranNum];
    display.textContent = prize[name];
    prize[quantity] -= 1;
    FOR (every prize of the onlineCoursePrize array):
        IF (prize.quantity === 0):
            LET currentIndex = onlineCoursePrize.indexOf(prize);
            onlineCoursePrize.slice(currentIndex, 1);
        END IF
    END FOR
END FUNCTION


function getRandomIntInclusive(min, max) {
  //Math.ceil() round up and returns the smallest integer gratern than or equal to a given number
  const minCeiled = Math.ceil(min);
  //Math.floor() round down the number and return the largest integer less than or equal to a given number
  const maxFloored = Math.floor(max);
  return Math.floor(Math.random() * (maxFloored - minCeiled + 1) + minCeiled); // The maximum is inclusive and the minimum is inclusive
}

Possible considerations:
    - The number will be reset if it is refresh
    - If the user click more than once, the stock will decrease by 1 more 

Question:
1. is the key of the object actually a variable?
2. How to create a table with javascript so that it will get updated every single time?
3. How to use settimeout with button?
4. What's the difference bewteen innerHTML and appendChild?
    append adds element objects (or text) while innerHTML parses HTML strings. They're two different methods/properties that are designed to do two different things. You can read more about their capabilities on MDN:
    - https://developer.mozilla.org/en-US/docs/Web/API/Element/append
    - https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML
5. How to clear table content with javascript? 
    FUNCTION deleteTableRow():
        FOR (i is equals to 0; when i is smaller than onlineCoursePrize.length; i increase by 1 after the iteration):
            prizeTable.deleteRow(i);
        END FOR
    END FUNCTION
    **This causes the problems of locating row index. When the first row is deleted, the second row became the first row. --> The problem of mutating a table with iterations

    Method 2: Use the last row index
    Is there a way to delete a row without needing to know its specific index number? Think about the relationship between a table and its last row. What if you just kept deleting the last one until there was only one left (the header)?

    WHILE (prizeTable.rows.length > 1):
        prizeTable.deleteRow(-1);
    END WHILE

6. How to find the total number of table rows?
    - Use rows.length method

7. How to storedata so that it does not get lost while retrieving it? 
    - Use the localStorage method
    - Steps involved:
        - On Page Load
            - Check if there's any prize data saved in localStorage
            - If yes, getItem() and JSON.parse() into the onlineCoursePrize variable
            - If no, use the default prize array

            IF (localStorage is true):
                onlineCoursePrize = JSON.parse(localStorage.getItem("onlineCoursePrize"));
            END IF

        - After evert clicked
            - immediately save the new version to local storage using setItem() and JSON.stringify()
            - localStorage.setItem("onlineCoursePrize", JSON.stringify(onlineCoursePrize));

    - How to check if a localStorage is empty?
        - Use the `localStorage.length` property
            if (localStorage.length === 0) {
                console.log("localStorage is empty.");
            } else {
                console.log("localStorage contains items.");
            }

8. How to clear local storage for a page
    - Use your keyboard's F12 key to open the Google Chrome Developer Tools Console. Click Application in the console's top menu. Expand the Local storage list located under the Storage section in the console's left menu. Right-click your site(s) and click Clear to delete the local storage.


Notes:
1. It is dangerous to remove items from an array while you are looping forward through it. The loop can get confused and skip elements.
The modern and safest way to remove items from an array is to not "remove" them at all. Instead, you create a new array containing only the items you want to keep. The .filter() method is perfect for this.
Here is how you would rewrite your entire checking logic:

