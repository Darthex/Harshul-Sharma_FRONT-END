# Q1. Explain what the simple List component does.
The List component is a React component that renders a list of items, where one item can be selected at a time. The list items are displayed as li elements with a background color that changes depending on whether the item is selected or not.

The WrappedSingleListItem component is a memoized functional component that renders a single list item. It takes four props: index, isSelected, onClickHandler, and text. index is the index of the current item in the list, isSelected is a boolean value indicating whether the item is currently selected or not, onClickHandler is a function that is called when the item is clicked, and text is the text content of the item. The isSelected prop is used to determine the background color of the li element.

The WrappedListComponent is another memoized functional component that takes an array of items as a prop. It uses the useState hook to keep track of the index of the currently selected item, which is initially set to null. The useEffect hook is used to reset the selected index to null whenever the items prop changes. The component also defines a handleClick function that updates the selected index when a list item is clicked. Finally, the component renders a list of SingleListItem components, passing the necessary props to each one.


# Q2. What problems / warnings are there with code?
There are a few problems/warnings with the code:

The items prop is defined as an array of objects with a text property, but the PropTypes definition for items is incorrect. It should be PropTypes.arrayOf(PropTypes.shape({ text: PropTypes.string.isRequired })).

The setSelectedIndex function in the WrappedListComponent is not used correctly. It should be just const [selectedIndex, setSelectedIndex] = useState(null);.

The onClick event handler for the li element in WrappedSingleListItem should be a function that is called when the item is clicked, rather than being called immediately.


# Q3. Please fix, optimize, and/or modify the component as much as you think is necessary.
Here's an optimized and modified version of the code that addresses the issues:

jsx
```jsx
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const SingleListItem = memo(({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
});

SingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

// List Component
const List = memo(({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items &&
        items.map((item, index) => (
          <SingleListItem
            key={index}
            onClickHandler={handleClick}
            text={item.text}
            index={index}
            isSelected={selectedIndex === index}
          />
        ))}
    </ul>
  );
});

List.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({ text: PropTypes.string.isRequired })
  ),
};

export default List;
```
