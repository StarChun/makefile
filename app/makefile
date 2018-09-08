#makefile

CC			:= gcc

TARGET		:= hello

SRCDIR		:= src
BUILDDIR	:= obj
TARGETDIR	:= bin

SRCEXT		:= c
DEPEXT		:= d
OBJEXT		:= o

CFLAGS		:= -Wall -O2
#LDFLAGS		:=
INC			:= -Isrc -Iinclude

#---------------------------------------------------------------------------------
# DO NOT EDIT BELOW THIS LINE
#---------------------------------------------------------------------------------

sources		:= $(shell find $(SRCDIR) -type f -name *.$(SRCEXT))
objects		:= $(patsubst $(SRCDIR)/%,$(BUILDDIR)/%,$(subst .$(SRCEXT),.$(OBJEXT),$(sources)))
dependencies	:= $(subst .$(OBJEXT),.$(DEPEXT),$(objects))

# Defauilt Make
all: directories $(TARGETDIR)/$(TARGET)

# Remake
remake: cleaner all

# ディレクトリ生成
directories:
	@mkdir -p $(TARGETDIR)
	@mkdir -p $(BUILDDIR)
	#@echo "$(shell find $(SRCDIR) -type f -name *.$(SRCEXT))"
	#@echo "$(subst .$(SRCEXT),.$(OBJEXT),$(sources))"
	#@echo "$(patsubst $(SRCDIR)/%,$(BUILDDIR)/%,$(subst .$(SRCEXT),.$(OBJEXT),$(sources)))"

# 中間生成物のためのディレクトリを削除
clean:
	$(RM) -rf $(BUILDDIR)

# 中間生成物のためのディレクトリと最終生成物のためのディレクトリを削除
cleaner: clean
	$(RM) -rf $(TARGETDIR)

# 自動抽出した.dファイルを読み込む
-include $(dependencies)

# オブジェクトファイルをリンクしてバイナリを生成
$(TARGETDIR)/$(TARGET): $(objects)
	$(CC) -o $(TARGETDIR)/$(TARGET) $^ $(LDFLAGS)

# ソースファイルのコンパイルしてオブジェクトファイルを生成
# また、ソースファイルの依存関係を自動抽出して.dファイルに保存
$(BUILDDIR)/%.$(OBJEXT): $(SRCDIR)/%.$(SRCEXT)
	mkdir -p $(dir $@)
	$(CC) $(CFLAGS) $(INC) -c -o $@ $<
	$(CC) $(CFLAGS) $(INC) -MM $(SRCDIR)/$*.$(SRCEXT) > $(BUILDDIR)/$*.$(DEPEXT)
	@cp -f $(BUILDDIR)/$*.$(DEPEXT) $(BUILDDIR)/$*.$(DEPEXT).tmp
	@sed -e 's|.*:|$(BUILDDIR)/$*.$(OBJEXT):|' < $(BUILDDIR)/$*.$(DEPEXT).tmp > $(BUILDDIR)/$*.$(DEPEXT)
	@sed -e 's/.*://' -e 's/\\$$//' < $(BUILDDIR)/$*.$(DEPEXT).tmp | fmt -1 | sed -e 's/^ *//' -e 's/$$/:/' >> $(BUILDDIR)/$*.$(DEPEXT)
	@rm -f $(BUILDDIR)/$*.$(DEPEXT).tmp

# Non-File Targets
.PHONY: all remake clean cleaner