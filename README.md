# st_tools

Tools for streamlit.

Persistent session_state across restarts.

## Installation

```python
pip install st-tools
```

## Usage

This section describes the tools available in `st-tools`.

### SessionStateManager

These examples show how to keep your session state persistent across
restarts.

#### Basic usage

```python
import streamlit as st
from st_tools import SessionStateManager

valid_values = [
    "",
    "choice 1",
    "choice 2",
    "choice 3",
    "choice 4",
]

# 1. Create a Session State Manager
#    with a main key, valid values,
#    and default:
config = SessionStateManager(
    instance_id="unique-name",
    main_key=("selected_item", "", valid_values),
    keys=[],
)

# 2. Use the main key, and save when changing:
selected_item = st.selectbox(
    "Pick item:",
    valid_values,
    key="selected_item",
    on_change=config.save
)
```

This will keep that last value for "selected_item", even if you
restart streamlit, or reopen browser tab.

#### Advanced usage

With this method, you can have different sets of session states based
on the main_key's associated value.

```python
import streamlit as st
from st_tools import SessionStateManager

valid_values = [
    "",
    "choice 1",
    "choice 2",
    "choice 3",
    "choice 4",
]

# 1. Create a Session State Manager
#    with a main key, valid values,
#    and default:
config = SessionStateManager(
    instance_id="unique-name",
    main_key=("selected_item", "", valid_values),
    keys=["pagesize"],
)


# 2. Use the main key, save it when changes:
selected_item = st.selectbox(
    "Pick item:",
    valid_values,
    key="selected_item",
    on_change=config.save
)

if selected_item:
    # 3. load the settings for the main-key-value:
    config.initialize({"pagesize": 20})

    # 4. Use the settings, save it when it changes
    pagesize = st.number_input(
        "Page size:", min_value=1, max_value=100, key="pagesize", on_change=config.save
    )
    st.write(pagesize)
```

In this example, there will be different sets of settings for each of the values of the main key, namely "choice 1", "choice 2", "choice 3", and "choice 4".

If you reload the page, it will keep the session_state keys.
