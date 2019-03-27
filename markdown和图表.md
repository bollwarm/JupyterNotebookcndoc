# markdown和图表
想没有想过一个场景，你的jupyter notebook是导出一个报告给业务人员看的，他们不想看到那些密密麻麻的代码，只想留下markdown和图表，在jupyter notebook加入下面这段代码就好

```
import IPython.core.display as di di.display_html（'<script>jQuery（function（）（if（jQuery（"body.notebook-_app"）.leng
```

```
import IPython.core.display as di;
di.display_html('<script>jQuery(function() {if (jQuery("body.notebook_app").length == 0) { jQuery(".input_area").toggle(); jQuery(".prompt").toggle();}});</script>', raw=True)
from IPython.display import HTML
HTML('''<script>
code_show=true; 
function code_toggle() {
if (code_show){
$('div.input').hide();
} else {
$('div.input').show();
}
code_show = !code_show
} 
$( document ).ready(code_toggle);
</script>
<form action="javascript:code_toggle()"><input type="submit" value="Click here to toggle on/off the raw code."></form>''')
```
