# Lingopy

Lingopy is a lightweight localization library for Python that helps you manage localized messages effortlessly.

## Features
- **Dynamic Language Selection**: Choose the current language dynamically using a callable function.
- **Fallback Language**: Define a fallback language for cases where the current language localization is not available.
- **Custom Exceptions**: Handle localization errors gracefully with a custom `LocalizationError` exception.
- **Flexible Usage**: Supports string concatenation and repetition for localized messages.

## Installation
You can include the `Lingopy` module in your project by simply copying the source code.

## Usage
### Basic Setup
First, define a language function that returns the current language code. Then, create an instance of `Lingopy`:
```python
lang_func = lambda: \'en\'
lingopy = Lingopy(lang_func=lang_func, fallback_lang=\'pl\')
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

### Accessing Localized Messages
You can retrieve the localized text using the `text` property:
```python
print(BAD_EMAIL_MSG.text)  # Outputs the message in the current language
print(BAD_PASSWORD_MSG.text)  # Outputs the message in the current language
```

### String Operations
Localized messages can be concatenated or repeated:
```python
print(BAD_EMAIL_MSG + BAD_PASSWORD_MSG)  # Concatenates the localized messages
print(BAD_EMAIL_MSG.text + " Added text")  # Concatenates string with localized message
print(BAD_EMAIL_MSG * 2)  # Repeats the localized message
```

### Handling Errors
If a localization is not found, a `LocalizationError` will be raised:
```python
try:
    print(lingopy.localized(en="Hello!").text)
except LocalizationError as e:
    print(e)  # Handle the error
```

## Custom Exceptions
### LocalizationError
This exception is raised when a localization is not found. You can handle it as follows:
```python
raise LocalizationError("Your error message here.")
```

## Conclusion
Lingopy makes it easy to manage localized messages in your applications, providing flexibility and clarity. For more advanced usage, you can explore modifying the `lang_func` and defining additional localizations as needed.

## License
This project is licensed under the MIT License.'