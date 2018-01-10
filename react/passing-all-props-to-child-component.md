# Passing all props to child component

React components supports spread syntax in jsx defined components. Allowing e.g. a container component to pass all its props to a child. Additional props can be added if desired.

```jsx

class ItemContainer extends Component {
  constructor(props){
      super(props);
      this.handleItem = this.handleItem.bind(this);
  }

  handleItem(){
      this.props.dispatch(someAction);
  }

  render(){
      return(
          <Item {this.props} handleItem={this.handleItem} >
      );
  }
}

ItemContainer.propTypes = {
  dispatch: PropTypes.func.isRequired,
  activeItem: PropTypes.number.isRequired,
  error: PropTypes.string.isRequired
};

const mapStateToProps = state => {
  return {
    activeItem: state.activeItem
    error: state.error,
  };
};

export default connect(mapStateToProps)(ItemContainer);

```