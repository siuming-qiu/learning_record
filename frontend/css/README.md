## 利用 :valid 和 :invalid 来做表单即时校验
```html
 <div class="container">
    <div class="row" style="margin-top: 2rem;">
      <form>
        <div class="form-group">
          <label>name</label>
          <input type="text" required placeholder="请输入名称">
        </div>
        <div class="form-group">
          <label>email</label>
          <input type="email" required placeholder="请输入邮箱">
        </div>
        <div class="form-group">
          <label>homepage</label>
          <input type="url" placeholder="请输入博客url">
        </div>
        <div class="form-group">
          <label>Comments</label>
          <textarea required></textarea>
        </div>
      </form>
    </div>
  </div>
```
```css
.valid {
  border-color: #429032;
  box-shadow: inset 5px 0 0 #429032;
}

.invalid {
  border-color: #D61D1D;
  box-shadow: inset 5px 0 0 #D61D1D;
}

.form-group {
  width: 32rem;
  padding: 1rem;
  border: 1px solid transparent;
  &:hover {
    border-color: #eee;
    transition: border .2s;
  }
  label {
    display: block;
    font-weight: normal;
  }
  input,
  textarea {
    display: block;
    width: 100%;
    line-height: 2rem;
    padding: .5rem .5rem .5rem 1rem;
    border: 1px solid #ccc;
    outline: none;
    &:valid {
      @extend .valid;
    }
    &:invalid {
      @extend .invalid;
    } 
  }
}

```