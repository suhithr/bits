project(
    'cpp',
    'cpp',
    default_options : ['cpp_std=c++14'],
    license : 'MIT',
    version : '0.0.1',
)

boost = dependency('boost', required : true, modules : ['system', 'thread'])
gtest = dependency('gtest', required : false)
gmock = dependency('gmock', required : false)
benchmark = dependency('benchmark', required : false)
threads = dependency('threads')

incdirs = include_directories('include')

lib = library(
     'bits',
     [
        'src/cacheline.cpp',
        'src/statics.cpp',
     ],
     dependencies : [boost, threads],
     include_directories : incdirs,
)

if gtest.found() and gmock.found()
    test = executable(
        'bits-test',
        [
            'test/main.cpp',
            'test/cacheline.cpp',
            'test/rcu.cpp',
            'test/tag_list.cpp',
        ],
        dependencies : [boost, gtest, gmock, threads],
        include_directories : incdirs,
        link_with : lib,
    )

    test('test', test)
endif

if benchmark.found()
    bench = executable(
        'bits-bench',
        [
            'bench/main.cpp',
            'bench/atomics.cpp',
            'bench/cacheeffects.cpp',
            'bench/dispatch.cpp',
            'bench/rcu.cpp',
            'bench/statics.cpp',
            'bench/syscall.cpp',
        ],
        dependencies : [boost, benchmark, threads],
        include_directories : incdirs,
        link_with : lib,
    )

    benchmark('bench', bench)
endif

bin_defs = {
    'cacheline'    : 'bin/cacheline.cpp',
    'hyperthreads' : 'bin/hyperthreads.cpp',
}

bin_exes = []

foreach name, cpp : bin_defs
    bin_exes += executable(
        name,
        [cpp],
        dependencies : [boost, threads],
        include_directories : incdirs,
        link_with : lib,
    )
endforeach

run_target('format', command : 'scripts/clang-format.sh')

