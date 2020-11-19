# Searchable SelectBox for Vue 3
## Search your values in selectbox with selectable_searchbox_vue_3 package

[Live example here](https://vue-3-searchable-selectbox.netlify.app)

```
   npm i searchable_selectbox_vue_3
```

```
Example
YourComponent.vue Script
<script>
import { ref } from "vue";
import SearchableSelectBox from "searchable_selectbox_vue_3";
export default {
  components: {
    SearchableSelectBox,
  },
  setup() {
    const examples = ref(null);
    window.addEventListener("scroll", (_) => {
      if (window.scrollY >= 300) {
        examples.value.style.alignItems = "center";
      } else {
        examples.value.style.alignItems = "flex-start";
      }
    });
    const currentValue = ref(null);
    const items = [
      {
        title: "İzmir",
        value: "izmir",
      },
      {
        title: "Paris",
        value: "paris",
      },
      {
        title: "New York",
        value: "new-york",
      },
      {
        title: "Roma",
        value: "roma",
      },
      {
        title: "Berlin",
        value: "berlin",
      },
      {
        title: "Tokyo",
        value: "tokyo",
      },
    ];
    const limit = 2;
    return {
      items,
      limit,
      currentValue,
      examples,
    };
  },
};
</script>
```

```
    YourComponent.vue Template
    {{ currentValue }}
    <div>
        <SearchableSelectBox
        @change-value="currentValue = $event"
        :items="items"
        ></SearchableSelectBox>
    </div>
    //Also you can pass the limit
    <div>
        <SearchableSelectBox
        :limit='5' Limit must be number
        @change-value="currentValue = $event"
        :items="items"
        ></SearchableSelectBox>
    </div>

```