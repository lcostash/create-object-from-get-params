# Create Object from GET parameters

With this simple function we can map GET parameters from the url into the Object.

```javascript
function createObjectFromGetParams(text) {
    const result = {};
    if (result.length === 0) return result;

    const params = text.split('&');
    params.forEach(param => {
        const [path, value] = param.split('=');
        const keys = path.split('.');
        let current = result;

        keys.forEach((key, index) => {
            if (index === keys.length - 1) {
                current[key] = decodeURIComponent(value);
            } else {
                if (!current[key]) current[key] = {};
                current = current[key];
            }
        });
    });

    return result;
}
```

For example, we have the next params:

```javascript
const url = 'user.name.firstname=John&user.name.lastname=Doe&user.role=Administrator&setup.theme=Space%20Gray&setup.language=English&filter=';
const obj = createObjectFromGetParams(url);
```

The results of ``console.log(obj);`` will be the next object:

```
{
  user: {
    name: { 
      firstname: 'John', 
      lastname: 'Doe'
    },
    role: 'Administrator'
  },
  setup: { 
    theme: 'Space Gray', 
    language: 'English'
  },
  filter: ''
}
```

The results of ``console.log(obj.user.name.firstname);`` will display:

```
John
```