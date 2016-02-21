---
Title: Vue.js Smart Table Component
layout: default
---

# {{page.title}}

In this article I'll outline how I build the smart table component in Vue.js. This is not a port from Angular, in fact I don't even know how the smart table in Angular is made.

Vue is a very lightweght but extremely powerful framework. In fact, it took me minutes to build a simple table component and you can see one in the [example page of the vue project](http://vuejs.org/examples/grid-component.html).

Then I figured out I could build a powerful general-purpose table component I could reuse here and there. I've done that and the result were really great. But there is a foundamental problem with these kind of tables.

> When you pass data to the table through a props,
> the table will display the data exactly as you pass it.
> This means there is no immediate way to tell the table **how** to render data.

Sure you could put built-in some options for example instead of passing the string `"http://google.com"` you could pass an object `{ href: "http://google.com", title: "Random search engine" }` but then you are limited no a certain number of options.

The smart table I built is a little more powerful. Say you want to display some cell as links. With the Smart Table you can write something like this

    {% highlight html %}
    <smart-table :body="body">
      <linker slot="link"></linker>
    </smart-table>
    {% endhighlight %}

Easy right?

Actually for this to work you are in charge of creating the linker component, but that's far from difficult

    {% highlight javascript %}
    Vue.extend({
      template: '<a href="{{value.href}}">{{value.title}}</a>',
      data () { return { value: { href: '#', title: 'default title' } } }
    })
    {% endhighlight %}

That's it, I even provided default data! The smart table would work provided that `link` is the name of the column you want to replace with your linker component. But how does it work? I'm going to post the component to the community as soon as possible, in the meantime you can take a look at the basic inner workings.

    {% highlight html %}
    <template>
      <div class="smart-table">
        <table>
          <tbody>
            <tr v-for="row in body">
              <td v-for="(col, value) in row" :class="col">
                <slot :name="col">
                  {{value}} // the default behavior is plain text
                </slot>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </template>
    {% endhighlight %}
    {% highlight javascript %}
    <script>
    export default {
      props: ['body'],
      ready () {
        let childrenQuantity = this.$children.length
        for (var i = 0; i < childrenQuantity; i++) {
          let row = this.body[i]
          let cols = Object.keys(row)
          let curChild = this.$children[i]
          let maybeCol = curChild.$el.attributes.getNamedItem('slot').value
          if (maybeCol !== undefined &&
             cols.indexOf(maybeCol) !== -1) {
               let col = maybeCol
               curChild.value = row[col]
          }
        }
      }
    }
    </script>
    {% endhighlight %}
