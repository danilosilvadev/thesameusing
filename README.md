# The same using...
The same using is a repo that create small apps and replicates the same using another tech. So let's have fun convertind things.

# TODO:

TODO using DOM and es2015:

**HTML**

```html
<body>
    <h1>Simple Todo List</h1>

    Name:
    <input id="formAdd" type="text" name="name">
    <button onclick="add()">Add</button>
    </br>
    <div id="todoList">
        <ul id="list">

        </ul>
    </div>

    <script src="todo.js"></script>
</body>
```

**JS:**

```js
createLiElement = (value) => {
    const ul = document.getElementById("list");
    const li = document.createElement("li");
    const children = ul.children.length + 1;
    const buttonDelete = document.createElement("button");
    const buttonUpdate = document.createElement("button");
    buttonDelete.innerHTML = "Delete";
    buttonDelete.onclick = deleteBtn;
    buttonUpdate.innerHTML = "Update";
    document.getElementById("formAdd").value = '';
    buttonUpdate.onclick = (e, addEventListener) => updateBtn(closure(), e);
    li.setAttribute("id", children);
    const closure = () => {
        return {
            ul: ul,
            li: li,
            buttonDelete: buttonDelete,
            buttonUpdate: buttonUpdate
        }
    }
    li.appendChild(document.createTextNode(value + ' '));
    li.appendChild(buttonDelete);
    li.appendChild(buttonUpdate);
    ul.appendChild(li)
};

add = () => {
    const addValue = document.getElementById("formAdd").value;
    return (addValue === null || addValue === '') ? alert('Please fill this form') : createLiElement(addValue);
};

deleteBtn = (e) => {
    const liId = e.path[1].id;
    const elem = document.getElementById(liId);
    elem.parentNode.removeChild(elem);
};

updateBtn = (closure, e) => {
    const value = document.getElementById("formAdd").value;    
    return (value === null || value === '') ? alert('Write in the field!') : (() => {
        const liId = e.path[1].id;
        const elem = document.getElementById(liId);
        closure.buttonDelete.innerHTML = "Delete";
        closure.buttonDelete.onclick = deleteBtn;
        closure.buttonUpdate.innerHTML = "Update";
        const div = document.createElement("div");
        const item = closure.li.appendChild(div);
        item.setAttribute("id", liId);        
        div.appendChild(document.createTextNode(value + " "))
        div.appendChild(closure.buttonDelete);
        div.appendChild(closure.buttonUpdate);
        elem.parentNode.replaceChild(item, elem);
        document.getElementById("formAdd").value = '';      
        })();
};
```

- 1 tsu - JQuery

```html
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script><!--Add this line to the html -->
```
```js
createLiElement = value => {
  const ul = $("#lista");
  const children = $("#lista li").length+1;
  const li = $("<li>").attr("id", children);
  const button = new buttons();
  button.buttonDelete.click(() => deleteBtn(button.buttonDelete));
  button.buttonUpdate.click(e => updateBtn(closure));
  const closure = {
    ul: ul,
    li: li,
    buttonDelete: button.buttonDelete,
    buttonUpdate: button.buttonUpdate
  }
  li.append(document.createTextNode(value + ' ')).append(button.buttonDelete).append(button.buttonUpdate);
  ul.append(li);
  $("#formAdd").val('');
};

buttons = function() {
  return {
    buttonDelete : $("<button>").text('delete'),
    buttonUpdate : $("<button>").text('update')
  }
};

add = () => {
  const addValue = $("#formAdd").val();
  return (addValue === null || addValue === '') ? alert('Please fill this form') : createLiElement(addValue);
};

deleteBtn = button => {
  button.parent().remove();
}

updateBtn = (closure) => {
  const value = $("#formAdd").val();
  return (value === null || value === '') ? alert('Write in the field!') : (() => {
    const itemOld = $('#'+closure.li.attr('id'));
    const itemNew = $('<li>').attr('id', closure.li.attr('id')).
    append(document.createTextNode(value + " ")).append(closure.buttonDelete).
    append(closure.buttonUpdate);
    itemOld.replaceWith(itemNew);
    $("#formAdd").val('');
  })();
};
```
# GET

```js
//This shows in the console random jokes about Chuck Norris.
(function loadDoc() {
      var xhttp = new XMLHttpRequest();
      xhttp.onreadystatechange = function() {
      if (this.readyState == 4 && this.status == 200) {
        var obj = JSON.parse(this.response);  //Parser to extract just the information that i want.
            console.log(obj.value.joke); 
        } else {
          console.log('error');
        }
      };
      xhttp.open("GET", "http://api.icndb.com/jokes/random", false);
      xhttp.send();
    } )();
```
- 1 tsu - JQuery:

```js
(function loadDoc() {
    $.get("http://api.icndb.com/jokes/random", function(data){console.log(data.value.joke); })
    .fail(function(){
        console.log('error');
    });
})();
```
# PAGINATION

```js
// This is a pagination Sample:

// The constructor takes in an array of items and a integer indicating how many
// items fit within a single page
function PaginationHelper(collection, itemsPerPage){
  this.collection = collection, this.itemsPerPage = itemsPerPage;
}

// returns the number of items within the entire collection
PaginationHelper.prototype.itemCount = function() {
  return this.collection.length;
}

// returns the number of pages
PaginationHelper.prototype.pageCount = function() {
  return Math.floor(this.collection.length/this.itemsPerPage)+1;
}

// returns the number of items on the current page. page_index is zero based.
// this method should return -1 for pageIndex values that are out of range
PaginationHelper.prototype.pageItemCount = function(pageIndex) {
  return pageIndex > this.pageCount() - 1 || pageIndex < 0 ?
  -1 : pageIndex !== this.pageCount() - 1 ?
  this.itemsPerPage : this.collection.length -
  (this.itemsPerPage*this.pageCount())
  + this.itemsPerPage;
}

// determines what page an item is on. Zero based indexes
// this method should return -1 for itemIndex values that are out of range
PaginationHelper.prototype.pageIndex = function(itemIndex) {
  return itemIndex >= this.collection.length || itemIndex < 0 ?
  -1 : itemIndex === 0 ? 0 : Math.round(++itemIndex/helper.itemsPerPage);
}

var helper = new PaginationHelper(['1',2,3,4,5,'6',7,8,9,10,'11',12,13,14,15,'16',17,18,19,20,'21', 22], 5);
console.log(helper.pageItemCount(4));
```
- 1 tsu - Using scope strategy

```js
const PaginationHelper = (collection, itemsPerPage) => {
  this.collection = collection;
  this.itemsPerPage = itemsPerPage;
  this.itemCount = () => this.collection.length;
  this.pageCount = () => Math.ceil(this.collection.length / this.itemsPerPage);
  this.pageItemCount = idx => {
    this.pageCount() === ++idx ? this.itemCount() % this.itemsPerPage :
    this.pageCount() < ++idx ? -1 : this.itemsPerPage;
  };
  this.pageIndex = idx => {
    if (idx < 0) return -1;
    return ++idx > this.itemCount() ? -1 : ++idx === this.itemCount() ? this.pageCount() - 1 :
    idx / this.itemsPerPage ^ 0;
  }
}
```
