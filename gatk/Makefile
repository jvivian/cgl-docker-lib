# Definitions
build_path = build/
runtime_path = runtime/
build_output = ${runtime_path}/gatk.jar
runtime_fullpath = $(realpath ${runtime_path})
build_tool = runtime-container.DONE
build_number ?= none
git_commit ?= $(shell git rev-parse HEAD)
nametag = computationalgenomicslab/gatk:3.4--src--unstable--${git_commit}--${build_number}

# Steps
all: ${build_output} ${build_tool}

${build_output}: ${build_path}/Dockerfile
	cd ${build_path} && docker build -t gatkbuild .
	docker run -v ${runtime_fullpath}:/data gatkbuild cp gatk.jar /data

${build_tool}: ${build_output} ${runtime_path}/Dockerfile
	cd ${runtime_path} && docker build -t ${nametag} .
	docker rmi -f gatkbuild
	touch ${build_tool}

push: all
	# Requires ~/.dockercfg
	docker push ${nametag}
