# React Component Code Review

## Introduction

Based on the code below answer the following queries:
1. Explain what the simple `List` component does.
Ans:- The List component renders a list of items passed in as an array in the items prop. Each item is displayed as a WrappedSingleListItem component, which takes in an index, isSelected, onClickHandler, and text prop. The index prop corresponds to the index of the item in the items array. The isSelected prop is a boolean that determines if the item is currently selected or not. The onClickHandler prop is a function that is called when the item is clicked, and it takes in the index prop as an argument. Finally, the text prop is the text that is displayed for the item.
2. What problems / warnings are there with code?
Ans:-1.The setSelectedIndex variable in the WrappedListComponent is not correctly set. It should be a state variable and should be declared using the useState hook instead of a variable declaration.

2.The isSelected prop in the WrappedSingleListItem component should be a boolean value but it is assigned with      selectedIndex, which is a state variable that is not boolean.

3.In the handleClick function in the WrappedListComponent, the index parameter is not being passed correctly to the onClickHandler prop. The correct way to pass it is by wrapping the function call in an anonymous function or using an arrow function.

4.The propTypes of the items prop in WrappedListComponent should be defined as PropTypes.arrayOf(PropTypes.shape({text: PropTypes.string.isRequired})) instead of PropTypes.array(PropTypes.shapeOf({text: PropTypes.string.isRequired})).
3. Please fix, optimize, and/or modify the component as much as you think is necessary.

## Code

```javascript

// import React, { useState, useEffect, memo } from 'react';
// import PropTypes from 'prop-types';

// // Single List Item
// const WrappedSingleListItem = ({
//   index,
//   isSelected,
//   onClickHandler,
//   text,
// }) => {
//   return (
//     <li
//       style={{ backgroundColor: isSelected ? 'green' : 'red'}}
//       onClick={onClickHandler(index)}
//     >
//       {text}
//     </li>
//   );
// };

// WrappedSingleListItem.propTypes = {
//   index: PropTypes.number,
//   isSelected: PropTypes.bool,
//   onClickHandler: PropTypes.func.isRequired,
//   text: PropTypes.string.isRequired,
// };

// const SingleListItem = memo(WrappedSingleListItem);

// // List Component
// const WrappedListComponent = ({
//   items,
// }) => {
//   const [setSelectedIndex, selectedIndex] = useState();

//   useEffect(() => {
//     setSelectedIndex(null);
//   }, [items]);

//   const handleClick = index => {
//     setSelectedIndex(index);
//   };

//   return (
//     <ul style={{ textAlign: 'left' }}>
//       {items.map((item, index) => (
//         <SingleListItem
//           onClickHandler={() => handleClick(index)}
//           text={item.text}
//           index={index}
//           isSelected={selectedIndex}
//         />
//       ))}
//     </ul>
//   )
// };

// WrappedListComponent.propTypes = {
//   items: PropTypes.array(PropTypes.shapeOf({
//     text: PropTypes.string.isRequired,
//   })),
// };

// WrappedListComponent.defaultProps = {
//   items: null,
// };

// const List = memo(WrappedListComponent);

// export default List;

  //  FIXED AND MODIFIED COMPONENT
  
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items && items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({text: PropTypes.string.isRequired})),
};

WrappedListComponent.defaultProps = {
  items: null,
};

const List = memo(WrappedListComponent);

export default List;



```