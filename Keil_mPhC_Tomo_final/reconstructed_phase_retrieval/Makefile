
### setting default paths of external programs
ITK?=/opt/ITK-CLIs/
ITKSNAP?=/opt/itksnap/

VGLRUN?=vglrun


SHELL:= /bin/bash

export PATH:= $(ITK)/bin:$(PATH)
export PATH:= $(ITKSNAP)/bin:$(PATH)


### check existance of external programs
## http://stackoverflow.com/questions/5618615/check-if-a-program-exists-from-a-makefile#25668869
EXECUTABLES = $(VGLRUN) extract_subimage anisoDiff-grad_f32 anisoDiff-curv_f32 itksnap
K:= $(foreach exec,$(EXECUTABLES),\
	$(if $(shell PATH=$(PATH) which $(exec)),some string,$(error "No $(exec) in PATH: $(PATH)")))



BASENAME = RPR



.PHONY : all clean 

all : $(BASENAME)_ROI_rsi+1+4.0_adc+150+2_seg.nii.gz


% :: %.gz # try decompress-first rule https://www.gnu.org/software/make/manual/html_node/Match_002dAnything-Rules.html#Match_002dAnything-Rules  https://www.gnu.org/software/make/manual/html_node/Double_002dColon.html#Double_002dColon
	unpigz -v -k -- $<

%.gz : %
	pigz -v -- $< 




%_rsi+1+4.0.mhd : %.mhd
	resample $< $@ 1 1 4.0 4.0 4.0

%_ROI.mhd : %.mhd
	extract_subimage $< $@  240 60 0  920 1130 2048


%_adg+50+2.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 50 0.0625 2.0 # tests for 1/2^(N+1) but docs and guide say 1/2^N: https://itk.org/pipermail/insight-users/2010-February/035268.html

%_adg+15+2.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 15 0.0625 2.0

%_adc+15+2.mhd : %.mhd
	anisoDiff-curv_f32 $< $@ 1 15 0.0625 2.0

%_adg+15+20.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 15 0.0625 20.0

%_adc+15+20.mhd : %.mhd
	anisoDiff-curv_f32 $< $@ 1 15 0.0625 20.0

%_adg+150+0.2.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 150 0.0625 0.2

%_adc+150+0.2.mhd : %.mhd
	anisoDiff-curv_f32 $< $@ 1 150 0.0625 0.2

%_adg+150+2.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 150 0.0625 2.0

%_adc+150+2.mhd : %.mhd
	anisoDiff-curv_f32 $< $@ 1 150 0.0625 2.0

%_adg+150+20.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 150 0.0625 20.0

%_adc+150+20.mhd : %.mhd
	anisoDiff-curv_f32 $< $@ 1 150 0.0625 20.0

%_adg+5+10.mhd : %.mhd
	anisoDiff-grad_f32 $< $@ 1 5 0.0625 10.0


%_seg.nii.gz : %.mhd $(BASENAME)_ROI_rsi+1+4.0.mhd
	 $(VGLRUN) itksnap -g $< -o $(lastword $^) # overlay as reference

#prevent removal of any intermediate files http://stackoverflow.com/questions/5426934/why-this-makefile-removes-my-goal https://www.gnu.org/software/make/manual/html_node/Chained-Rules.html
.SECONDARY: 

