# What is the use of the ownProps arg in mapStateToProps and mapDispatchToProps?

If the `ownProps` parameter is specified, react-redux will pass the props that were passed to the component into your `connect` functions. So, if you use a connected component like this:

    import ConnectedComponent from './containers/ConnectedComponent'
    
    <ConnectedComponent
      value="example"
    />
    

The `ownProps` inside your `mapStateToProps` and `mapDispatchToProps` functions will be an object:

    { value: 'example' }
    

And you could use this object to decide what to return from those functions.

* * *

For example, on a blog post component:

    // BlogPost.js
    export default function BlogPost (props) {
      return <div>
        <h2>{props.title}</h2>
        <p>{props.content}</p>
        <button onClick={props.editBlogPost}>Edit</button>
      </div>
    }
    

You could return Redux action creators that do something to that specific post:

    // BlogPostContainer.js
    import { bindActionCreators } from 'redux'
    import { connect } from 'react-redux'
    import BlogPost from './BlogPost.js'
    import * as actions from './actions.js'
    
    const mapStateToProps = (state, props) =>
      // Get blog post data from the store for this blog post ID.
      getBlogPostData(state, props.id)
    
    const mapDispatchToProps = (dispatch, props) => bindActionCreators({
      // Pass the blog post ID to the action creator automatically, so
      // the wrapped blog post component can simply call `props.editBlogPost()`:
      editBlogPost: () => actions.editBlogPost(props.id)
    }, dispatch)
    
    const BlogPostContainer = connect(mapStateToProps, mapDispatchToProps)(BlogPost)
    export default BlogPostContainer
    

Now you would use this component like so:

    import BlogPostContainer from './BlogPostContainer.js'
    
    <BlogPostContainer id={1} />
