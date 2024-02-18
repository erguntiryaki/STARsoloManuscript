SHELL=/bin/bash

all: install-python-venv salmon_1.4.0 STAR_2.7.9a generateDecoyTranscriptome.sh mashmap bedtools bwa alevin-fry_0.1.0 kbpy_0.25.0 cellranger-5.0.1 gffread

include ../Mf.pars

# Target to ensure Python3 and venv are installed
install-python-venv:
	@echo "Ensuring Python3 and venv are installed..."
	sudo apt-get update && sudo apt-get install -y python3-venv
	@echo "Python3 and venv are ensured."

# Function to create a Python venv and install packages
define create_venv_and_install
	mkdir -p $(1) &&\
	python3 -m venv $(1)/venv &&\
	. $(1)/venv/bin/activate &&\
	pip install $(2) &&\
	deactivate
endef

salmon_%:
	mkdir -p $@_distr && cd $@_distr && \
	wget https://github.com/COMBINE-lab/salmon/releases/download/v$*/salmon-$*_linux_x86_64.tar.gz && \
	tar xvfz salmon-$*_linux_x86_64.tar.gz && \
	cd ../ && ln -s `find ./salmon_$*_distr/ -name salmon` $@

bwa:
	sudo apt-get update
	sudo apt-get install -y zlib1g-dev bwa

STAR_%:
	curl -o STAR_$* https://raw.githubusercontent.com/alexdobin/STAR/$*/bin/Linux_x86_64_static/STAR && \
	chmod u+x STAR_$*

kbpy_0.25.0:
	$(call create_venv_and_install,$@,"kb-python==0.25.0")

generateDecoyTranscriptome.sh:
	wget https://github.com/COMBINE-lab/SalmonTools/raw/master/scripts/generateDecoyTranscriptome.sh &&\
	chmod u+x generateDecoyTranscriptome.sh

#alevin-fry_0.1.0:
#	$(call create_venv_and_install,$@,"alevin-fry==0.1.0 -c bioconda")

# Target for installing Rust and Cargo, if not already installed
install-rust:
	@echo "Installing Rust and Cargo..."
	@curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
	@python -c "import os; os.environ['PATH'] += ':/root/.cargo/bin'; print('Modified PATH:', os.environ['PATH'])"
	@echo "Rust and Cargo installed successfully."
	
# Target for installing alevin-fry using Cargo
alevin-fry_0.1.0: install-rust
	@echo "Installing alevin-fry..."
	@/bin/bash -c "source $$HOME/.cargo/env && cargo install alevin-fry"
	@echo "alevin-fry installed successfully."

cellranger-5.0.1:
	wget http://labshare.cshl.edu/shares/dobin/dobin/STARsolo/Preprint/10X/cellranger-5.0.1.tar.gz &&\
	tar xvfz cellranger-5.0.1.tar.gz && 'rm' -f cellranger-5.0.1.tar.gz

mashmap:
	wget https://github.com/marbl/MashMap/releases/download/v2.0/mashmap-Linux64-v2.0.tar.gz &&\
	tar xvfz mashmap-Linux64-v2.0.tar.gz && 'rm' -f mashmap-Linux64-v2.0.tar.gz &&\
	ln -s mashmap-Linux64-v2.0/mashmap

bedtools:
	wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary &&\
	chmod u+x bedtools.static.binary &&\
	ln -s bedtools.static.binary bedtools

gffread:
	wget https://github.com/gpertea/gffread/releases/download/v0.12.6/gffread-0.12.6.Linux_x86_64.tar.gz &&\
	tar xvfz gffread-0.12.6.Linux_x86_64.tar.gz &&\
	'rm' -f gffread-0.12.6.Linux_x86_64.tar.gz &&\
	ln -s gffread-0.12.6.Linux_x86_64/gffread .
