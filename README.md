# Perl-Scripts
#This script aimed at calculating the numbers of reads that >100k,100k-80k,80k-60k,&lt;60k and extract their corresponding sequences.

#!/usr/bin/perl -w
use strict;
use warnings;

if(@ARGV !=6){
  print "Usage: perl $0 xx.fa b100k.txt 100_80k.txt 80_60k.txt l60k.txt stat_out.txt\n";
  print "\nAttention: This script aimed at calculating the numbers of reads that >100k,100k-80k,80k-60k,<60k and extract their corresponding sequences\n";
  print "\n##############################################################################\n";
}

my %seq_hash;
open(IN,"$ARGV[0]")||die $!;
my $id;
while(<IN>){
  chomp;
  if(/^>/){
    $id = $_;
  }
  else {
    $seq_hash{$id} .= $_;
  }
}
close IN;

my $a = 0;
my $b = 0;
my $c = 0;
my $d = 0;
my $total = 0;

open(OUT1,">$ARGV[1]")||die $!;
open(OUT2,">$ARGV[2]")||die $!;
open(OUT3,">$ARGV[3]")||die $!;
open(OUT4,">$ARGV[4]")||die $!;
foreach (keys %seq_hash){
  $total +=1;
  my $len = length($seq_hash{$_});
  if ($len >= 100000){
    $a += 1;
    #open(OUT1,">>$ARGV[1]")||die $!;
    print OUT1 "$_\n$seq_hash{$_}\n";
  }
  elsif ($len < 100000 && $len >= 80000){
    $b += 1;
    #open(OUT2,">>$ARGV[2]")||die $!;
    print OUT2 "$_\n$seq_hash{$_}\n";
  }
  elsif ($len < 80000 && $len >= 60000){
    $c += 1;
    #open(OUT3,">>$ARGV[3]")||die $!;
    print OUT3 "$_\n$seq_hash{$_}\n";
  }
  else{
    $d += 1;
    #open(OUT4,">>$ARGV[4]")||die $!;
    print OUT4 "$_\n$seq_hash{$_}\n";
  }
}
close OUT1;
close OUT2;
close OUT3;
close OUT4;

open(STAT,">$ARGV[5]")||die $!;
print STAT "Size\t>100k\t100k-80k\t80k-60k\t<60k\tTotal\n";
print STAT "Num\t$a\t$b\t$c\t$d\t$total\n";
close STAT;
