Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-24T02:41:16+02:00

====== customizing form builders ======
Created Tuesday 24 September 2013

[ ] make a form helper that calls form_tag with appropriate bootstrap class and builder: CustomBuilder
[ ] make custom builder for tagging form items if needed:
class LabellingFormBuilder < ActionView::Helpers::FormBuilder
  def text_field(attribute, options={})
    label(attribute) + super
  end
end


http://thepugautomatic.com/2013/06/helpers/ - concat and capture
