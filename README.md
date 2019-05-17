# BEM

This repostory is an improved [BEM](http://getbem.com/ "BEM — Block Element Modifier") implement compared to [the one](https://github.com/ElementUI/theme-chalk/blob/master/src/mixins/mixins.scss "theme-chalk/mixins.scss at master · ElementUI/theme-chalk") [Element-UI](https://github.com/elemefe/element "ElemeFE/element: A Vue.js 2.0 UI Toolkit for Web") used. It writes more natural and can batter handle edge conditions.

## Comparison

* Normal

	```scss
	@include b(human) {
	  @include e(finger) {
	    @include m(little) {
	      foo: bar;
	    }
	  }
	}
	```

	Element:

	```css
	.el-human__finger--little {
	  foo: bar;
	}
	```

	This:

	```css
	.el-human__finger--little {
	  foo: bar;
	}
	```

* Case.1 - Nesting Element inside modifier

	```scss
	@include b(human) {
	  @include m(male) {
	    @include e(leg) {
	      foo: bar;
	    }
	  }
	}
	```

	Element:

	```css
	.el-human--male .el-human__leg {
	  foo: bar;
	}
	```

	This:

	```css
	.el-human--male .el-human__leg {
	  foo: bar;
	}
	```

* Case.2 - Nesting Element inside pseudo-class or pseudo-element

	```scss
	@include b(human) {
	  &:hover {
	    @include e(hand) {
	      foo: bar;
	    }
	  }
	}
	```

	Element:

	```css
	.el-human:hover .el-human__hand {
	  foo: bar;
	}
	```

	This:

	```css
	.el-human:hover .el-human__hand {
	  foo: bar;
	}
	```

* Case.3 - Nesting Element inside state class

	```scss
	@include b(human) {
	  &.is-hurt {
	    @include e(head) {
	      foo: bar;
	    }
	  }
	}
	```

	Element:

	```css
	.el-human.is-hurt .el-human__head {
	  foo: bar;
	}
	```

	This:

	```css
	.el-human.is-hurt .el-human__head {
	  foo: bar;
	}
	```

* Case.4 - Arbitrary CSS combinators

	```scss
	// Not recommand
	@include b(human) {
	  @include e(arm) {
	    &:focus {
	      $outer: &;

	      @include b(human) {
	        @include e(arm) {
	          @include m(left) {
	            #{$outer}+& {
	              foo: bar;
	            }
	          }
	        }
	      }
	    }

	    $outer: &;

	    @include b(human) {
	      @include e(hand) {
	        @include m(right) {
	          #{$outer}~& {
	            foo: bar;
	          }
	        }
	      }
	    }
	  }
	}
	```

	Element:

	```css
	.el-human__arm:focus + .el-human__arm:focus .el-human  .el-human__arm--left {
	  foo: bar;
	}

	.el-human__arm ~ .el-human__hand--right {
	  foo: bar;
	}
	```

	This:

	```css
	.el-human__arm:focus + .el-human__arm:focus .el-human__arm--left {
	  foo: bar;
	}

	.el-human__arm ~ .el-human__arm .el-human__hand--right {
	  foo: bar;
	}
	```

* Additional case.1 - Nesting another class

	```scss
	@include b(human) {
	  .mutated {
	    @include e(head) {
	      foo: bar;
	    }
	  }
	}
	```

	Element:

	```css
	.el-human__head {
	  foo: bar;
	}
	```

	This:

	```css
	.el-human .mutated .el-human__head {
	  foo: bar;
	}
	```

* Additional case.2 - Top level Element

	```scss
	@include b(human) {
	  @include e(head) {
	    foo: bar;
	  }
	}

	@include e(body) {
	  foo: bar;
	}
	```

	Element:

	```css
	.el-human__head {
	  foo: bar;
	}

	.el-human__body {
	  foo: bar;
	}
	```

	This:

	```bash
	Error: Base-level rules cannot contain an element mixin
			on line 24 of mixins.scss, in mixin `e`
			from line 9 of test.scss
	>>
	@error "Base-level rules cannot contain an element mixin";
	-----------^
	```

## Reference

[SASS: 简单点，写 BEM 的方式简单点](https://zhuanlan.zhihu.com/p/28650879 "SASS: 简单点，写 BEM 的方式简单点 - 知乎")
