---
layout: post
title:      "React/Redux Final Project"
date:       2019-05-24 16:20:05 +0000
permalink:  react_redux_final_project
---

For my final final project I chose to create an app called Spacious, which fetches data from the National Park Service’s API and returns a list of parks filtered by state and allows the user to add parks to their favorites.

It was easy enough to get started by using create-react-app. I decided to have one repo for both front and backend. I found it interesting to have two servers wired up to the same project, and to go back and forth between the two when I wanted to check my json data or interact with the frontend.

I fell in love with semantic ui, which proved to be a lot friendlier than bootstrap in my case. The documentation was easy to understand and awesomely interactive, and it provided me with some sleek styling that I love.

My understanding of Redux was really put to the test and to be completely honest, I don’t think I really understood the purpose of Redux until I was halfway through the project. At some point, the light bulb went off and what flipped the switch for me was the connect function. While going through the Redux labs, I had the idea that connect was somehow sending props from one component to another. And while it is possible to utilize connect in order to send props from one component to another, what it’s really doing is connecting the component that it’s called in to the redux store where ALL the app’s data is stored. THIS is what makes it a step ahead of plain React, where each component only has access to it’s own state or the state passed down to it by a parent component. Lots of little states here and there. Redux comes through and stores all of the required state in one place, and by connecting any of your components, grants access to the entire app.

I’ll include an instance from my project that utilizes the connect function here:

```
import React, { Component } from 'react'
import {connect} from 'react-redux'
import { selectPark } from '../actions/parkActions'
import '../App.css'
import NoDataHeader from '../components/NoDataHeader'
import { bindActionCreators } from 'redux'

class AllParks extends Component {

  render() {

    console.log(this.props)
    if (this.props.allParks && this.props.currentUser) {
      return (
        <div className="ui list">
        {this.props.allParks.map((park, index) =>
          <div className="item" key={park.parkCode}>
            <div className="content">
              <i className="caret up icon"></i>
              <a className="header" onClick={() => this.props.selectPark(park)}>{park.name}</a>
              <div className="description">{park.description.substring(0, 75)}...</div>
            </div>
          </div>
        )}
        </div>
      )
    }
    else {
      return <div><NoDataHeader /></div>
    }
  }
}

const mapStateToProps = (state) => {
  return {
    allParks: state.parks.allParks,
    selectedState: state.parks.selectedState,
    currentUser: state.user.currentUser
  }
}

const mapDispatchToProps = (dispatch) => bindActionCreators({
  selectPark
}, dispatch)

export default connect(mapStateToProps, mapDispatchToProps)(AllParks)

```

I’ve included my AllParks class component above, which renders the data returned from an external API. In order to do that, it needs access to the Redux store for state items like allParks, selectedState, and currentUser, as well as my selectPark action creator. By using connect on the last line after export default, I’m telling Redux to allow access to those things stored in the higher state to this connected component. The AllParks component accesses the Redux store items by calling this.props.whateverStateOrDispatchIsCalled. This understanding in conjunction with the Redux DevTools helped me to stop being intimidated by Redux and rather to admire it for its complicated simplicity. Here’s the link to my repo: https://github.com/brittanygrebnova/Spacious

Thanks for reading!
