REPOROOT=../../..
# Use make help, to see the available rules

include $(REPOROOT)/.make.defaults

clean:: .defaults.clean

venv:: .defaults.ray-lib-src-venv
	@# Help: Install requirements from all transforms into a venv 
	. ./venv/bin/activate && bash ./prepare_env.sh

jupyter::
	@# Help: Activate the venv and start the example notebook  
	. ./venv/bin/activate && . ./launch.sh
