---
layout: post
title:      "[Flat]-Ironing out React-Redux Basics"
date:       2020-08-17 00:09:07 +0000
permalink:  flat_-ironing_out_react-redux_basics
---


I can't believe I'm submitting my final Flatiron project today. It feels like yesterday I was blogging about why I wanted to be a software engineer, and here I am about to begin my job hunting journey. But before the job hunt begins, I must take a moment to bask in the afterglow of my React-Redux project. This project was a lot of fun to work on given the basic minimum requirements and flexibility to really make it our own.

For my project, I decided to make a community events web app where Oakland residents can visit and stay up to date on upcoming events. The events fall into categories (Volunteering, Health & Wellness, Activism, to name a few...), and users have the ability to add events or categories to the page.

Working through the code wasn't too challenging, but as I got into the thick of my coding, I realized I had overlooked a super basic (and *important*) piece of information about React: the difference between containers and components. 

As I was finishing up my code, I realized my components and containers were a bit of a mess. My final bit of work included completely re-organizing my code. Components should be totally stateless (which means not importing connect to the file or mapping state to props). Any outside information needed in components must be passed in as props from a parent file. Originally, I had a CategoriesContainer (which maps through all of my categories and then renders the categories on the CategoryCard file) and a separate CategoriesForm. While reorganizing my code, I decided to move most of my functions and logic having to do with the form from the form file to the CategoriesContainer file, and then move the form itself into a CategoryForm component. This cleaned up my code and made the file structure easier to follow. After restructuring, my CategoriesContainer file renders the following:

```
render() {
        const categories = this.props.categories.map(category => <CategoryCard key={category.id} category={category} getCategories={this.props.getCategories} deleteCategory={this.handleDelete} />)
    return (
        <>
            <hr />
                <div className="container">
                {this.props.categories.loadingCategories ? <h5>Loading......</h5> : categories}
                </div>
            <hr />
            <div className="category-form">
                {this.state.showForm === false ? <button onClick={this.handleClick}>Add a Category</button> : <CategoryForm handleSubmit={this.handleSubmit} handleChange={this.handleChange}/>}
            </div>
        </>
    );
    }
}
```

...while my CategoryForm file looks like this:

```
const CategoryForm = (props) =>
    
    <>
        <form onSubmit={props.handleSubmit}>
            <input type="text" name="name" onChange={props.handleChange}></input>
            <input type="submit" />
        </form>
    </>

export default CategoryForm;
```

This is so much cleaner and easier to follow. Currently, my event form is in a container since I currently have the form popping up as its own page (via route /events/new). I'm going to continue working on this app this week and try to move the event form to the category show page - if I'm successful at this, I'll re-locate the events form to a component to once again clean up the code and stick with this familiar structure.

Onward!
