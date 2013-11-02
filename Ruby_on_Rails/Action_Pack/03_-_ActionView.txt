Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-09-18T13:11:05+02:00

====== 03 - ActionView ======
Created Wednesday 18 September 2013

* debug() - prints object in yaml

===== the template environment =====
	* instance variables defined in controller
	* controller-defined objects - flash, headers, logger, params, request, response, session (except flash, shouldn't be accessed directly in view)
	* controller object - accessible via controller() method - can call any public method defined by controller
	* base_path - path to base templates directory

===== multibutton form =====
http://railscasts.com/episodes/38-multibutton-form
# view
<% if params[:preview_button] %>
  <%= textilize @project.description %>
<% end %>
...
<%= submit_tag 'Create' %>
<%= submit_tag 'Preview', :name => 'preview_button' %>

# controller
def create
  @project = Project.new(params[:project])
  if params[:preview_button] || !@project.save
    render :action => 'new'
  else
    flash[:notice] = "Successfully created project."
    redirect_to project_path(@project)
  end
end


http://railscasts.com/episodes/258-token-fields-revised - **completion of multiple items in text field** - e.g. mail recepients from contact list/history

http://railscasts.com/episodes/416-form-objects
