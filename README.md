# Lingopy

Lingopy is a lightweight localization library for Python that helps you manage localized messages effortlessly.

## Features

-   **Dynamic Language Selection**: Choose the current language dynamically using a callable function.
-   **Fallback Language**: Define a fallback language for cases where the current language localization is not available.
-   **Custom Exceptions**: Handle localization errors gracefully with a custom `LocalizationError` exception.
-   **Flexible Usage**: Supports string concatenation and repetition for localized messages.

## Installation

To install `Lingopy`, you can use the following command in your terminal:

```bash
pip install lingopy
```

## Usage

### Basic Setup

First, define a language function that returns the current language code. Then, create an instance of `Lingopy`:

```python
from lingopy import Lingopy

lang_func = lambda: "en"
lingopy = Lingopy(lang_func=lang_func, fallback_lang="pl")
```

### Localizing Messages

You can create localized messages by passing key-value pairs to the `localized` method:

```python
BAD_EMAIL_MSG = lingopy.localized(
    en="You have entered the bad e-mail.",
    pl="Wpisałeś zły adres e-mail.",
)

BAD_PASSWORD_MSG = lingopy.localized(
    en="You have entered the bad password.",
    pl="Wpisałeś złe hasło."
)
```

You can also localize messages without accessing the Lingopy (Lingopy is the configuration base for the localized strings).

```python
from lingopy import Localized

BAD_EMAIL_MSG = Localized(
    lang_func=lambda: 'en',
    fallback_lang='pl',
    en="You have entered the bad e-mail.",
    pl="Wpisałeś zły adres e-mail.",
)

BAD_PASSWORD_MSG = Localized(
    lang_func=lambda: 'en',
    fallback_lang='pl',
    en="You have entered the bad password.",
    pl="Wpisałeś złe hasło."
)
```

### Accessing Localized Messages

You can retrieve the localized text using:

-   The `text` property:

```python
print(BAD_EMAIL_MSG.text)  # Outputs the message in the current language
print(BAD_PASSWORD_MSG.text)  # Outputs the message in the current language
```

-   By calling it:

```python
print(BAD_EMAIL_MSG())  # Outputs the message in the current language
print(BAD_PASSWORD_MSG())  # Outputs the message in the current language
```

-   By applying `str()` or `__str__()` to it:

```python
print(str(BAD_EMAIL_MSG))  # Outputs the message in the current language
print(BAD_PASSWORD_MSG.__str__())  # Outputs the message in the current language
```

-   **_Sometimes_** just using the variable will work.

```python
print(BAD_EMAIL_MSG)  # Outputs the message in the current language. Works because `print` automatically uses __str__ on printed values.
print(BAD_PASSWORD_MSG)  # Outputs the message in the current language. Works because `print` automatically uses __str__ on printed values.
```

### String Operations

Localized messages can be concatenated or repeated:

```python
print(BAD_EMAIL_MSG + BAD_PASSWORD_MSG)  # Concatenates the localized messages
print(BAD_EMAIL_MSG + " Added text")  # Concatenates localized message with string
print(BAD_EMAIL_MSG.text + " Added text")  # Concatenates localized message with string
print(BAD_EMAIL_MSG * 2)  # Repeats the localized message

# Will raise error:
print("Added text" + BAD_EMAIL_MSG)  # String class does not handle adding other type than `str` to it.
```

### Advanced example with `lang_func`.

The `lang_func` in Lingopy is a callable that returns the current language code. This design allows for dynamic language selection based on the context of your application, making it especially useful in web applications.

Example: Using `lang_func` with Flask.
In a Flask application, you might want to determine the user's preferred language based on HTTP request headers. For instance, you can read the `Accept-Language` header, which is commonly used by browsers to indicate the user's language preferences.

Here's how you can set it up:

1. **Set up your Flask application**:

    ```python
    from flask import Flask, request

    from lingopy import Lingopy

    app = Flask(__name__)


    # Define the language function
    def get_language():
        # Get the 'Accept-Language' header from the request
        lang = request.headers.get('Accept-Language')

        # Return None when header is not presented.
        if not lang:
            return None

        # Split the header values into a list
        languages = lang.split(',')

        # Remove duplicates while maintaining order
        unique_languages = []

        for language in languages:
            # Simplify the language code (e.g., 'pl-PL' => 'pl')
            simplified_language = language.split('-')[0]

            # Add to the list only if it's not already included
            if simplified_language not in unique_languages:
                unique_languages.append(simplified_language)

        # Return the first result or default to 'en'
        return unique_languages[0] if unique_languages else None


    # Create an instance of Lingopy
    lingopy = Lingopy(lang_func=get_language, fallback_lang="pl")


    @app.route('/')
    def home():
        # Localized messages
        welcome_msg = lingopy.localized(
            en="Welcome to the application!", pl="Witamy w aplikacji!"
        )

        return welcome_msg.text  # Returns the message in the user's language


    if __name__ == "__main__":
        app.run(debug=True)
    ```

2. **Accessing Localized Messages**:
   When a user accesses your application, the `get_language` function will determine the language based on the `Accept-Language` header sent by their browser. If the preferred language is not available, the `Lingopy` instance will fall back to the specified fallback language (in this case, Polish).

3. **Handling Edge Cases**:
   If the user's browser does not specify a preferred language, or if the specified language is not supported, the application will gracefully return the message in the fallback language.

This approach allows you to create a more user-friendly application that respects the user's language preferences, making your application accessible to a broader audience.

## Conclusion

Lingopy makes it easy to manage localized messages in your applications, providing flexibility and clarity. For more advanced usage, you can explore modifying the `lang_func` and defining additional localizations as needed.

## License

This project is licensed under the MIT License.
