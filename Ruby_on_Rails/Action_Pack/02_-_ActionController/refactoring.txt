Content-Type: text/x-zim-wiki
Wiki-Format: zim 0.4
Creation-Date: 2013-11-02T11:35:06+01:00

====== refactoring ======
Created Saturday 02 November 2013

* refactoring permitted (strong) params into a class
'''
class PermittedParams < Struct.new(:params, :user)
  def topic
    params.require(:topic).permit(*topic_attributes)
  end

  def topic_attributes
    if user && user.admin?
      [:name, :sticky]
    else
      [:name]
    end
  end
end
'''
	* controller:
'''
def permitted_params
  @permitted_params ||= PermittedParams.new(params, current_user)
end
helper_method :permitted_params
'''

	* view (show only if current user can change this param)
''<% if permitted_params.topic_attributes.include? :sticky %>''
