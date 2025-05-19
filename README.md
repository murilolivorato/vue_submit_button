# Creating a Submit Button Component with Vue.js

A modern, reusable submit button component for Vue.js applications featuring loading states, customizable styles, and Bulma CSS integration.



<p align="center"><br><br>
<img src="https://miro.medium.com/v2/resize:fit:700/1*fnStwJ26LL734tJSuztpYA.png" alt="Login Page" /><br><br>
</p>

more information at -
https://medium.com/@murilolivorato/create-a-submit-button-component-with-vue-js-3d8b9fdb5c43

## Overview

This project demonstrates how to build a versatile submit button component that:
- Provides visual feedback during form submission
- Integrates seamlessly with Bulma CSS
- Offers customizable styling options
- Supports various button states
- Implements accessibility best practices

## Features

- Loading state animation
- Customizable button styles
- Bulma CSS integration
- Disabled state handling
- Form submission integration
- Accessibility support
- Responsive design
- SCSS support

## Prerequisites

- Node.js (v14 or higher)
- Vue.js 3.x
- npm or yarn
- Basic understanding of Vue.js components
- Basic knowledge of SCSS

## Installation

1. Create a new Vue project:
```bash
npm create vue@latest
```

2. Navigate to project directory:
```bash
cd your-project-name
```

3. Install dependencies:
```bash
# Install Bulma and SASS
npm install bulma
npm install -D sass
```

## Project Structure
```bash
src/
├── components/
│ ├── SubmitButton/
│ │ └── SubmitButton.vue
│ └── examples/
│ └── SubmitButtonExample.vue
├── assets/
│ └── styles/
│ └── submit-button.scss
└── App.vue
```


## Implementation

### SubmitButton Component
```vue
<!-- src/components/SubmitButton/SubmitButton.vue -->
<template>
  <button
    :class="[
      'button',
      `is-${type}`,
      `is-${size}`,
      { 'is-loading': loading },
      { 'is-fullwidth': fullWidth }
    ]"
    :disabled="disabled || loading"
    @click="handleClick"
    type="submit"
  >
    <span class="icon" v-if="loading">
      <i class="fas fa-spinner fa-spin"></i>
    </span>
    <span v-if="!loading">{{ label }}</span>
    <span v-else>{{ loadingLabel }}</span>
  </button>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  label: {
    type: String,
    default: 'Submit'
  },
  loadingLabel: {
    type: String,
    default: 'Loading...'
  },
  type: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'success', 'danger', 'warning', 'info'].includes(value)
  },
  size: {
    type: String,
    default: 'normal',
    validator: (value) => ['small', 'normal', 'medium', 'large'].includes(value)
  },
  loading: {
    type: Boolean,
    default: false
  },
  disabled: {
    type: Boolean,
    default: false
  },
  fullWidth: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['click'])

const handleClick = (event) => {
  if (!props.loading && !props.disabled) {
    emit('click', event)
  }
}
</script>

<style lang="scss" scoped>
.button {
  position: relative;
  transition: all 0.2s ease;

  &.is-loading {
    cursor: wait;
    
    .icon {
      margin-right: 0.5rem;
    }
  }

  &:not(.is-loading):hover {
    transform: translateY(-1px);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  &:not(.is-loading):active {
    transform: translateY(0);
    box-shadow: none;
  }
}

// Custom button types
.is-primary {
  background-color: #485fc7;
  color: white;
  
  &:hover {
    background-color: darken(#485fc7, 5%);
  }
}

.is-success {
  background-color: #48c774;
  color: white;
  
  &:hover {
    background-color: darken(#48c774, 5%);
  }
}

// Add more custom button types as needed
</style>
```

## Usage

### Basic Implementation
```vue
<!-- src/components/examples/SubmitButtonExample.vue -->
<template>
  <form @submit.prevent="handleSubmit">
    <div class="field">
      <SubmitButton
        :loading="isLoading"
        label="Save Changes"
        loading-label="Saving..."
        type="primary"
        @click="handleSubmit"
      />
    </div>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import SubmitButton from '../SubmitButton/SubmitButton.vue'

const isLoading = ref(false)

const handleSubmit = async () => {
  isLoading.value = true
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 2000))
    console.log('Form submitted successfully')
  } catch (error) {
    console.error('Error submitting form:', error)
  } finally {
    isLoading.value = false
  }
}
</script>
```

### Form Integration with Validation
```vue
<template>
  <form @submit.prevent="handleSubmit">
    <div class="field">
      <label class="label">Email</label>
      <input
        v-model="formData.email"
        type="email"
        class="input"
        :class="{ 'is-danger': errors.email }"
      />
      <p class="help is-danger" v-if="errors.email">{{ errors.email }}</p>
    </div>

    <SubmitButton
      :loading="isLoading"
      :disabled="!isFormValid"
      label="Subscribe"
      loading-label="Subscribing..."
      type="success"
      @click="handleSubmit"
    />
  </form>
</template>

<script setup>
import { ref, computed } from 'vue'
import SubmitButton from '../SubmitButton/SubmitButton.vue'

const formData = ref({
  email: ''
})

const errors = ref({})
const isLoading = ref(false)

const isFormValid = computed(() => {
  return formData.value.email && !errors.value.email
})

const validateForm = () => {
  errors.value = {}
  
  if (!formData.value.email) {
    errors.value.email = 'Email is required'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.value.email)) {
    errors.value.email = 'Invalid email format'
  }
}

const handleSubmit = async () => {
  validateForm()
  
  if (isFormValid.value) {
    isLoading.value = true
    try {
      // Submit form logic
      await submitForm(formData.value)
    } finally {
      isLoading.value = false
    }
  }
}
</script>
```

## Props

### SubmitButton Component Props
- `label`: String (default: 'Submit') - Button text
- `loadingLabel`: String (default: 'Loading...') - Text shown during loading state
- `type`: String (default: 'primary') - Button style type
  - Options: 'primary', 'success', 'danger', 'warning', 'info'
- `size`: String (default: 'normal') - Button size
  - Options: 'small', 'normal', 'medium', 'large'
- `loading`: Boolean (default: false) - Loading state
- `disabled`: Boolean (default: false) - Disabled state
- `fullWidth`: Boolean (default: false) - Full width button

## Events

### SubmitButton Component Events
- `click`: Emitted when button is clicked (if not loading or disabled)

## Customization

### Styling
The component can be customized using SCSS variables:

```scss
// src/assets/styles/_variables.scss
$button-transition: 0.2s ease;
$button-hover-transform: translateY(-1px);
$button-hover-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

// Custom button types
$button-types: (
  'primary': #485fc7,
  'success': #48c774,
  'danger': #f14668,
  'warning': #ffdd57,
  'info': #3298dc
);
```

### Custom Classes
You can extend the component's styling by adding custom classes:

```vue
<SubmitButton
  class="custom-submit-button"
  :class="{ 'is-rounded': isRounded }"
  ...
/>
```

## Best Practices

1. **Accessibility**
   - Use proper button types
   - Implement loading states
   - Provide clear visual feedback
   - Use appropriate ARIA attributes
   - Ensure proper contrast

2. **Performance**
   - Use proper event handling
   - Implement proper prop validation
   - Use appropriate component structure

3. **User Experience**
   - Provide clear loading states
   - Implement smooth transitions
   - Handle all interaction states
   - Ensure responsive design


## 👥 Author

For questions, suggestions, or collaboration:
- **Author**: Murilo Livorato
- **GitHub**: [murilolivorato](https://github.com/murilolivorato)
- **linkedIn**: https://www.linkedin.com/in/murilo-livorato-80985a4a/

## 🙏 Acknowledgments

- FastAPI documentation
- React documentation
- Kubernetes documentation
- Docker documentation
- Open source community

## 📸 Screenshots

### Button
![Button](https://miro.medium.com/v2/resize:fit:538/1*sxQtwsA96me_IZrRcY5iqg.png)

### Button Clicked
![Button Clicked](https://miro.medium.com/v2/resize:fit:538/1*mdpZsavvJyD8uLdNO28iug.png)


<div align="center">
  <h3>⭐ Star This Repository ⭐</h3>
  <p>Your support helps us improve and maintain this project!</p>
  <a href="https://github.com/murilolivorato/vue_submit_button/stargazers">
    <img src="https://img.shields.io/github/stars/murilolivorato/vue_submit_button?style=social" alt="GitHub Stars">
  </a>
</div>
