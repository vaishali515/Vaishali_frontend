# Vaishali_frontend
1.) Explain what the simple List component does?
    The simple List component is a React functional component that receives an array of items as a prop, maps over the items to render a list of Single List Items, and 
    tracks the currently selected item's index.





2.) What problems / warnings are there with code?
   *   In 'WrappedSingleListItem' , the onClick event handler was incorrect. We need to wrap the 
     callback in an arrow function to prevent it from being executed immediately. We also need to 
      pass the index argument to the onClickHandler function using an arrow function
    * In 'WrappedListIComponent' , the state hook 'selectedIndex' wasb not properly 
       destructured. We also need to add a key prop when mapping the items array to prevent React from issuing a warning.
    * In the propTypes definition for 'WrappedListComponent' , ProTypes.array should be changed to PropTypes.arrayOf and PropTypes.shapeOf should be changed to 
       PropTypes.shape
    * In the defaultProps definition for 'WrappedListComponent', i provide a default items value as an array with some sample objects .





3.) Here is the optimize code.!
 
 
 
 
 
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
      {items.map((item, index) => (
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
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: [
    { text: 'A' },
    { text: 'B' },

  ],
};

const List = memo(WrappedListComponent);

export default List;


